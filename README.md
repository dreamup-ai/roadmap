# DreamUp.ai
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
- optional scale-to-zero: save money by scaling infrequently used models down to 0 when they arent being used
- Cloud-agnostic: let users follow the lowest gpu prices easily
- Can be run locally: Users should be able to run the full stack locally, provided they have adequate hardware
- Support an arbitrary number of models and pipelines
- Fully utilize available GPU resources: Binpack models into available vram, minimize idle time
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

### Hey, this isn't cloud agnostic!

Good eye! You've noticed some AWS specific services in the architecture diagram. In it's current form, DreamUp runs predominantly on AWS, and partially on [Salad](https://salad.com/). Additionally, our current team is overwhelmingly more experienced in AWS than in other clouds. There are several factors that mitigate tight coupling with AWS in this design:

- We wrap SQS in a typescript interface that abstracts its functionalities. It is simple to build queue "plugins" for other queueing services, such as RabbitMQ, that comply with this interface.
- We're using DynamoDB because it's fast, cheap, and very simple to operate and scale. However, none of the data storage and retrieval requirements of the platform are particularly complex, and swapping out the database is not a terribly complicated operation. The intention is to release variants of the API services that are intended for various database backends. i.e. One that supports DynamoDB, one that supports MongoDB, one that supports Postgres, etc. The priority of this development will be driven by community engagement.
- We're using S3 because we're already in AWS, but any s3-compatible bucket storage could be used out of the box. S3 interactions are constrained to the image service, which also will simplify releasing versions that support other bucket storage vendors.
- We're using Elastic File System (EFS) becaue we're already in AWS, but it is mounted to inference containers as normal network-attached storage, which is available from a variety of vendors, and not particularly complicated to set up on-premise either.

## API Services

![service model](images/Dreamup%20Service%20Model.png)

- [User Service](https://github.com/dreamup-ai/user-service)
- [Pipeline Service](https://github.com/dreamup-ai/pipeline-service)

### Auth Paradigm

- **server-to-server** communication is done via signed payloads with public-key cryptography. When one dreamup service communicates with another, it creates a signature of the payload using a private key, and includes that signature in a header. The receiving service uses a public key to verify the signature matches the payload and comes from an internal service.

- **user-to-service** communication is done via a jwt session, either in a cookie or a bearer token, that is signed with a private key, and validated with a public key. Only the **user service** issues sessions, and also hosts the public key at `/.well-known/session-jwks.json`, so any other service may verify the sessions.

## License

All of our code is released under the MIT license, available per-repo. Some dependencies may have other permissive licenses such as Apache. Components are chosen with commercial use in mind.
