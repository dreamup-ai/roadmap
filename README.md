# roadmap
This is a public repo for managing the engineering roadmap for dreamup.ai's open source platform release

## Design goals

- Arbitrarily scalable: should work for small and large services alike
- Cloud-agnostic: let users follow the lowest gpu prices easily
- Can be run locally: Users should be able to run the full stack locally, provided they have adequate hardware
- Good performance on low power devices
- Code re-use between web, mobile, and desktop apps
- Support an arbitrary number of models and pipelines
- Fully utilize available VRAM: Load multiple models per GPU worker if VRAM allows
- Support login via any OAuth2 provider
- Support 3rd party integrations

## Architecture

![Architecture Diagram](images/Dreamup%20v2%20architecture.png)