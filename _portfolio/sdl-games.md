---
title: "SDL Games"
excerpt: "Multiple remakes of games in SDL for the class, ITP 380"
order: 3
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/sdl-header.png
    teaser: /assets/images/sdl-teaser.png
gallery:
  - url: assets/images/frogger.gif
    image_path: assets/images/frogger.gif
    alt: "Frogger"
  - url: assets/images/mario.gif
    image_path: assets/images/mario.gif
    alt: "Mario"
  - url: assets/images/pacman.gif
    image_path: assets/images/pacman.gif
    alt: "PacMan"
---

Currently, the GitHub repository is private so that students currently taking the course can't find it and copy from it. If you are interested in viewing the repository, feel free to reach out! 

Multiple games made using SDL and C++ with starter code provided for some from class. Over the course of the semester, games built off of code from games made previously. The games adapted as part of the class include: 
- Pong
- Asteroids
- Frogger
- Mario
- Pac-Man
- Zelda
- Star Fox Tunnel
- Mario Kart
- Runner’s Edge (a first person 3D platformer) 
- Robotron 2084

Taking ITP 380 at USC was my first exposure to programming for video games and helped me learn some of the basics for game programming, writing quality code, and some other mathematical applications. Throughout the labs, we focused on using a component model for things like movement, sprites, and collisions while also having the ability to override functions if need be in our game object class, Actor.

Below are some examples of code snippets I personally enjoyed coding:

### PacMan Ghost Chase Behavior
The isolated logic for each ghost's chase state behaviors:
<div style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;">

```cpp
void GhostAI::UpdateChaseTarget()
{
  switch (mGhost->GetType())
  {
  case Ghost::Blinky:
    BlinkyChase();
    break;
  case Ghost::Pinky:
    PinkyChase();
    break;
  case Ghost::Inky:
    InkyChase();
    break;
  case Ghost::Clyde:
    ClydeChase();
    break;
  }
}

void GhostAI::BlinkyChase()
{
  // Blinky aggressively chases PacMan, targeting his previous node.
  PathNode* prevNode = gGame.GetPlayer()->GetPrevNode();
  if (prevNode->GetType() != PathNode::Tunnel)
  {
    mTargetNode = prevNode;
    return;
  }
  // Targets the node closest to PacMan when the previous node is a tunnel
  const Vector2& pacmanPosition = gGame.GetPlayer()->GetTransform().GetPosition();
  mTargetNode = GetClosestDefaultNode(pacmanPosition);
}

void GhostAI::PinkyChase()
{
  // Pinky tries to cut off PacMan, targeting the node 80 units in front of him.
  Vector2 targetPosition = gGame.GetPlayer()->GetPointInFrontOf(80.0f);
  mTargetNode = GetClosestDefaultNode(targetPosition);
}

void GhostAI::InkyChase()
{
  // Inky tries to be parallel and ahead of Blinky in order to pincer PacMan
  auto& ghosts = gGame.GetGhosts();
  // Doubles the vector from Blinky to a point 40 units in front of PacMan
  // to get Inky's target node
  Vector2 pointP = gGame.GetPlayer()->GetPointInFrontOf(40.0f);
  Vector2 v = pointP - ghosts[0]->GetTransform().GetPosition();
  Vector2 q = v * 2.0f + ghosts[0]->GetTransform().GetPosition();
  mTargetNode = GetClosestDefaultNode(q);
}

void GhostAI::ClydeChase()
{
  // Clyde copies Blinky's behavior when far away from PacMan (>150 units)
  float distanceToPacMan = Vector2::Distance(gGame.GetPlayer()->GetTransform().GetPosition(),
                        mGhost->GetTransform().GetPosition());
  if (distanceToPacMan > CLYDE_AGGRO_DISTANCE)
  {
    BlinkyChase();
    return;
  }
  mTargetNode = mGhost->GetScatterNode();
}

PathNode* GhostAI::GetClosestDefaultNode(const Vector2& position) const
{
  std::vector<PathNode*>& allNodes = gGame.GetPathNodes();
  PathNode* closestNode = nullptr;
  float minDistance = std::numeric_limits<float>::max();
  for (PathNode* node : allNodes)
  {
    if (node->GetType() == PathNode::Default)
    {
      float dist = Vector2::Distance(node->GetTransform().GetPosition(), position);
      if (dist < minDistance)
      {
        minDistance = dist;
        closestNode = node;
      }
    }
  }
  return closestNode;
}
```

</div>

{% include gallery %}


