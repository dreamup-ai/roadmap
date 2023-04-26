# roadmap
This is a public repo for managing the engineering roadmap for dreamup.ai's open source platform release

## What is it?

The **DreamUp AI platform** is an open-source, self-hosted inference-as-a-service (IaaS) platform designed to be cost-effective, scalable, cloud-agnostic, and easy to deploy.

The **DreamUp UI component collection** is an open-source set of react components designed with the goal of making AI-powered apps easier to build. Inspired by Gradio, but as react components that can be used with the dreamup api or on their own.

## Why not use 3rd party IaaS?

- **Cost**: The cost-per-inference is is much higher on managed APIs than in a well utilized DreamUp deployment.
- **Data Privacy**: Many users may be reluctant to, or even prohibited from sending proprietary or sensitive data to third party APIs. The DreamUp platform can be deployed inside your VPC, inside regulated clouds (GovCloud, etc), on-premise, or even locally. No trusting third parties required.
- **Proprietary/Custom Models**: Easily deploy your own models at scale while maintaing full control of your data.
- **Portability/Flexibility**: Deploy to any cloud, or all of them. Easily move expensive inference work to wherever gpu prices are cheapest.

## Who are we, and why are we doing this?

Dreamup.ai, a project by the Foundation for Technology in the Arts, is a small team of committed volunteers with decades of combined industry experience in companies large and small. We believe generative AI technology has the potential to dramatically revolutionize most, if not all, industries. We also believe it is critical that access to such great power is equitably distributed, and to that end we operate [DreamUp.ai](https://dreamup.ai), a free site for Stable Diffusion powered image generation. As a free platform run by volunteers with no outside investment, building a reliable, cost-effective, easy-to-operate inference platform is non-negotiable. As a nonprofit, we want to maximize our positive impact by also releasing the backend platform for free.

## Platform Design goals

- Arbitrarily scalable: should work for small and large services alike
- Cloud-agnostic: let users follow the lowest gpu prices easily
- Can be run locally: Users should be able to run the full stack locally, provided they have adequate hardware
- Support an arbitrary number of models and pipelines
- Fully utilize available VRAM: Load multiple models per GPU worker if VRAM allows
- Support login via any OAuth2 provider
- Support 3rd party integrations
- Use technology-agnostic standards like OpenAPI, JSON Schema,Terraform, Docker Compose

## UI component design goals

- Good performance on low power devices
- Code re-use between web, mobile, and desktop apps
- Can be dynamically composed from pipeline schemas
- Unstyled or very easy to style
- Small build size

## Architecture

![Architecture Diagram](images/Dreamup%20v2%20architecture.png)

## License

All of our code is released under the MIT license, available per-repo. Some dependencies may have other permissive licenses such as Apache. Components are chosen with commercial use in mind.
