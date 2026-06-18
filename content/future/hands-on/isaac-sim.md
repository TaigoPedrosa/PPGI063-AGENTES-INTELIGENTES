# NVIDIA Isaac Sim

---

## What it is

Physics-based robot simulation platform built on NVIDIA Omniverse. The industry-standard free tool for developing, simulating, and testing AI-driven robots in virtual environments before deploying to real hardware.

Open-source (since Isaac Sim 5.0, mid-2025).

---

## Key concepts

- **Digital twin:** create a virtual replica of your physical robot and environment
- **Sim-to-real transfer:** train policies in simulation, deploy directly on hardware
- **Synthetic data generation:** use Replicator to create labeled datasets for computer vision
- **GPU-accelerated physics:** PhysX engine on GPU enables massively parallel simulation

---

## Hardware requirements

- NVIDIA GPU (RTX recommended, minimum RTX 2070)
- Available as:
  - NGC container (local)
  - Cloud instance (Brev, AWS, GCP)
  - Standalone Omniverse application with GUI

---

## Access options

### 1. NGC container (local)

```bash
docker pull nvcr.io/nvidia/isaac-sim:latest
docker run --gpus all -it nvcr.io/nvidia/isaac-sim:latest
```

### 2. Cloud (Brev)
One-click GPU instances pre-configured with Isaac Sim.

### 3. Standalone application
Full Omniverse application with GUI, download from https://developer.nvidia.com/isaac/sim

---

## Key workflows

### Isaac Lab: GPU-accelerated robot learning
- Train RL policies with thousands of parallel environments
- Supports PPO, SAC, and custom algorithms
- Integrates with popular RL libraries

### Isaac TeleOp: demonstration collection
- Collect human demonstrations in simulation via teleoperation
- Use demonstrations to train imitation learning policies

### Replicator: synthetic data generation
- Generate labeled images for object detection, segmentation, pose estimation
- Randomize lighting, textures, camera angles, and object placement

### ROS 2 integration
- Connect simulated robots to ROS 2 nodes
- Test your full software stack in simulation before deployment
- Supports sensors: cameras, LiDAR, IMUs, contact sensors

---

## Related models

| Model | What | Link |
|---|---|---|
| **GR00T N1.6** | Open VLA model for humanoid robots | [Blog](https://developer.nvidia.com/blog/building-generalist-humanoid-capabilities-with-nvidia-isaac-gr00t-n1-6-using-a-sim-to-real-workflow/) |
| **Cosmos 3** | World model for generating training scenarios | [Guia Cosmos 3](cosmos3.md) |
| **Isaac Manipulator** | Pre-built manipulation skills | [Documentation](https://docs.omniverse.nvidia.com/isaacsim/latest/) |

---

## Videos and demos

| What | URL |
|---|---|
| Isaac Sim overview | https://developer.nvidia.com/isaac/sim |
| Isaac Lab for robot learning | https://research.nvidia.com/publication/2025-09_isaac-lab |
| GR00T N1.6 sim-to-real workflow | https://developer.nvidia.com/blog/building-generalist-humanoid-capabilities-with-nvidia-isaac-gr00t-n1-6-using-a-sim-to-real-workflow/ |
| Universal Robots case study | https://www.nvidia.com/en-us/customer-stories/universal-robots-accelerates-cobot-development-with-nvidia/ |
| RoboDK + Isaac Sim bridge | https://robodk.com/blog/robodk-bridges-nvidia-isaacsim-real-factory-floor/ |

---

## Links

- Main page: https://developer.nvidia.com/isaac/sim
- Documentation: https://docs.omniverse.nvidia.com/isaacsim/latest/
- NGC catalog: https://catalog.ngc.nvidia.com
- Isaac Lab: https://research.nvidia.com/publication/2025-09_isaac-lab

---

## Related pages

- **Context:** [Best Practices](../best-practices.md) section 7 lists embodied agents and generalist robotics as a frontier
- **Ecosystem:** Isaac Sim is how you would deploy policies from V-JEPA 2, Cosmos 3, or DreamerV3 on simulated robots before moving to real hardware
- **Industrial:** see [Hands-On](../hands-on.md) (BMW, Universal Robots, Tesla use NVIDIA simulation)
