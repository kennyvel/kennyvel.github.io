---
title: "Instant Messaging App"
excerpt: "Messaging App inspired by Discord"
order: 1
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/chat-app-header.png
    teaser: /assets/images/chat-app-teaser.png
    actions:
    - label: "<i class='fab fa-github'></i> GitHub Repo"
      icon: "fab fa-fw fa-github"
      url: https://github.com/comp550-spring26-csuci/Networked_Chat_App
---

This project was done together with 3 other people to learn more about how instant messaging works for applications like Discord. When working on this app we followed an agile framework, with 3 sprints lasting one month each with in-person scrum poker, sprint reviews, and professor feedback sessions. 

For this project, we made:
- Account registration and login.
- Early UI/UX mockups using Figma.
- Real-time updates for messaging and online statuses with SignalR.
- Notification badges from SignalR events
- Modular frontend React components for individual and group direct message rooms.
- A friend's list sidebar with live statuses and the ability to change your own status.
- Permissions for group DM owners to add and remove users from their group.
- Syncing across multiple applications logged into the same account

This project was very challenging as a lot of the tools and software we elected to use were new to us. We learned a lot about both the software and working as a group in an agile environment. Many tasks were split into user stories, assigned fibonacci points, and added into a GitHub kanban board so our sprints were organized. Here is a short demonstration of our application and snippets that I enjoyed coding:

### Demo Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/sEq0tTUZgzI?si=B836A9tEWJSxkdNQ&amp;start=138" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Friend's List Networking Handlers
React handlers for SignalR events when adding/removing friends, being added/removed, and status changes.

```js
useEffect(() => {
  const connection = getConnection();		

  const addedAsAFriendHandler = (chatEvent) => {
    const friendship = chatEvent.friendship;
    const addedFriend = currentUser.userId === friendship.affectedUser.userId 
                      ? friendship.initiatingUser : friendship.affectedUser;

    const friend = {
      id: addedFriend.userId,
      username: addedFriend.username,
      status: addedFriend.presenceStatus,
      customStatus: addedFriend.customStatusText ?? ""
    };

    setFriends(prev => [...prev, friend]);

    if(addedFriend === friendship.affectedUser) {
      setSuccessMsg(`${addedFriend.username} added successfully!`);
    }
    else {
      setSuccessMsg(`${addedFriend.username} added you!`);
    }
    setAddInput("");
    setTimeout(() => setSuccessMsg(""), 3000); 
  }
  const removedAsAFriendHandler = (chatEvent) => {
    const friendship = chatEvent.friendshipDeleted;
    const removedFriend = currentUser.userId === friendship.affectedUser.userId 
                      ? friendship.initiatingUser : friendship.affectedUser;

    setFriends(prev =>
      prev.filter(f => f.id !== removedFriend.userId)
    );

    if(removedFriend === friendship.affectedUser) {
      setSuccessMsg(`${removedFriend.username} unadded successfully!`);
    }
    else {
      setSuccessMsg(`${removedFriend.username} unadded you!`);
    }
    setAddInput("");
    setTimeout(() => setSuccessMsg(""), 3000); 
  }
  const changingStatusHandler = (chatEvent) => {
    const userStatus = chatEvent.userStatus;

    console.log(userStatus);

    setMyStatus(userStatus.state);
    setActiveStatus(userStatus.state);
    setMyCustomText(userStatus.customText ?? "");
    setActiveCustomText(userStatus.customText ?? "");

    setStatusFeedback("Status updated successfully!");
    setTimeout(() => setStatusFeedback(""), 3000);
  }
  const friendChangedStatusHandler = (chatEvent) => {
    const userStatus = chatEvent.userStatus;

    console.log(userStatus);

    setFriends(prev =>
      prev.map(friend =>
      friend.id === userStatus.userId
        ? {
          ...friend,
          status: userStatus.state,
          customStatus: userStatus.customText ?? ""
        }
        : friend
      )
    );
  };

  connection.on("FriendshipAdded", addedAsAFriendHandler);
  connection.on("FriendshipDeleted", removedAsAFriendHandler);
  connection.on("MyUserStatusChanged", changingStatusHandler);
  connection.on("FriendUserStatusChanged", friendChangedStatusHandler);

  if (currentUser?.userId) {
    console.log("CONNECTION STATE: ", connection.state);
    fetchMyProfile();
  }

  return () => {
    connection.off("FriendshipAdded", addedAsAFriendHandler);
    connection.off("FriendshipDeleted", removedAsAFriendHandler);
    connection.off("MyUserStatusChanged", changingStatusHandler);
    connection.off("FriendUserStatusChanged", friendChangedStatusHandler);
  };
}, []);
```
{: style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;"}

### Direct Message Network Handlers
React handlers for SignalR events when adding/removing notifications and adding/removing chat rooms. 
The SignalR events could be invoked by the user (i.e. viewing unread messages or creating a chat room) or by other users (i.e. sending a message or kicking someone from a chat room).

```js
useEffect(() => {
  const connection = getConnection();

  if (!connection) {
    console.warn("No SignalR connection yet");
    return;
  }

  const notificationHandler = (testMessagePreview) => {
    const messagePreview = testMessagePreview.messagePreview;
    if (messagePreview.senderId !== currentUser.userId &&
      selectedDMRef.current?.id !== messagePreview.chatRoomId) {

      setDms(prev =>
        prev.map(dm =>
          dm.id === messagePreview.chatRoomId
            ? {
              ...dm,
              unreadCount: (dm.unreadCount ?? 0) + 1
            }
            : dm
        )
      );

      setNotificationCount(prev => prev + 1);
    }
  };

  const membershipAddedHandler = (chatEvent) => {
    const room = chatEvent.room;
    const newRoom = { 
      id: room.id, 
      name: room.name
    };
              
    // update dm list to include the chatroom you were added to
    setDms(prev => [...prev, newRoom]);
    //setSelectedDM(newRoom); removed so only the creator focuses into the DM

    setRoomMembersMap(prev => ({
      ...prev,
      [newRoom.id]: [{ username: currentUser?.username || "You", userId: currentUser.userId }]
    }));
  }

  const removeDMFromList = (roomId) => {
    // update dm list by removing the chatroom you were removed from
    // Remove the room from your frontend DM list
    setDms(prev => prev.filter(dm => dm.id !== roomId));
    
    // Deselect the room if you are currently looking at it
    setSelectedDM(prev => prev?.id === roomId ? null : prev);
    
    // Clean up the memory map so it doesn't take up space
    setRoomMembersMap(prev => {
      const newMap = { ...prev };
      delete newMap[roomId];
      return newMap;
    });
  }

  const membershipRevokedHandler = (chatEvent) => {
    const room = chatEvent.chatGroupMembershipDeleted;
    removeDMFromList(room.id);
  }

  const groupDeletedHandler = (chatEvent) => {
    const room = chatEvent.chatGroupDeleted;
    removeDMFromList(room.id);
  }

  const markedAsReadHandler = (TestChatRoomRead) => {
    // See: TestChatRoomRead > ChatRoomRead
    const chatRoomRead = TestChatRoomRead.chatRoomRead;
    // Render your local unread count for the associated room being zero
    const roomId = chatRoomRead.id;

    const removedCount =
    dmsRef.current.find(dm => dm.id === roomId)?.unreadCount || 0;

    setDms(prev =>
      prev.map(dm =>
      dm.id === roomId
        ? { ...dm, unreadCount: 0 }
        : dm
      )
    );

    if(removedCount > 0) {
      setNotificationCount(prevTotal => prevTotal - removedCount);
    }
  }

  connection.on("ReceiveMessagePreview", notificationHandler);
  connection.on("ChatGroupMembershipAdded", membershipAddedHandler);
  connection.on("ChatGroupMembershipDeleted", membershipRevokedHandler);
  connection.on("ChatGroupDeleted", groupDeletedHandler);
  connection.on("ReceiveMarkedAsRead", markedAsReadHandler);

  return () => {
    connection.off("ReceiveMessagePreview", notificationHandler);
    connection.off("ChatGroupMembershipAdded", membershipAddedHandler);
    connection.off("ChatGroupMembershipDeleted", membershipRevokedHandler);
    connection.off("ChatGroupDeleted", groupDeletedHandler);
    connection.off("ReceiveMarkedAsRead", markedAsReadHandler);
  };
}, []);
```
{: style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;"}

### SignalR Connection Helper File
File that contains helper functions for managing the SignalR connection.

```js
import * as signalR from "@microsoft/signalr";

const accessToken = "YOUR_JWT_TOKEN";
export const TUNNEL_URL = "https://sslk8rt0-7081.usw3.devtunnels.ms";
let connection;

export function getConnection() {
    return connection;
}

// Used to gate application from loading if there is no SignalR connection
export async function ensureSignalRConnection() {
    if (connection && connection.state === signalR.HubConnectionState.Connected) {
        return connection;
    }

    if (connection && connection.state === signalR.HubConnectionState.Connecting) {
        return connection;
    }

    if (!connection) {
        connection = new signalR.HubConnectionBuilder()
            .withUrl(`${TUNNEL_URL}/chathub`, {
                accessTokenFactory: () => localStorage.getItem("access_token")
            })
            .withAutomaticReconnect()
            .build();

        connection.onclose(async () => {
            console.log("SignalR disconnected — retrying...");
            setTimeout(() => ensureSignalRConnection(), 2000);
        });
    }

    try {
        await connection.start();
        console.log("SignalR connected (or reconnected)");
        return connection;
    } catch (err) {
        console.error("SignalR start failed:", err);

        // retry loop
        setTimeout(() => ensureSignalRConnection(), 3000);
    }
}

export function startSignalRConnection() {
  console.log(`TOKEN in startSignalRConnection: ${localStorage.getItem("access_token")}`);

  connection = new signalR.HubConnectionBuilder()
    .withUrl(`${TUNNEL_URL}/chathub`, {
      accessTokenFactory: () => localStorage.getItem("access_token")
    })
    .withAutomaticReconnect()
    .build();

  return connection.start()
    .then(() => console.log("SignalR connected"))
    .catch(err => console.error("SignalR error:", err));
}

export function joinChatRoom(chatRoomId) {
    if (!connection || connection.state !== signalR.HubConnectionState.Connected) {
        console.warn("SignalR not connected yet");
        return;
    }

    return connection.invoke(
        "JoinChatRoom", { 
            performChatRoomAction : { 
                chatRoomId: chatRoomId 
            } 
        }
    );
}

export function leaveChatRoom(chatRoomId) {
    if (!connection || connection.state !== signalR.HubConnectionState.Connected) {
        console.warn("SignalR not connected yet");
        return;
    }

    return connection.invoke(
        "LeaveChatRoom", {
            performChatRoomAction : { 
                chatRoomId: chatRoomId 
            } 
        }
    );
}

export function sendMessage(chatRoomId, content, username) {
    return connection.invoke("SendMessageToChatRoom", {
        SendMessageToChatRoom: {
            ChatRoomId: chatRoomId,
            Content: content
        },
        SenderUsername: username
    });
}
```
{: style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;"}