# AWS WordPress Infrastructure Automation with Auto Scaling

This project demonstrates an end-to-end automated deployment and monitoring of a scalable WordPress instance on AWS, using CloudFormation, Auto Scaling Groups, Lambda, and Health Checks.

## Features

- Fully automated WordPress deployment using CloudFormation.
- Custom AMI creation and usage for rapid EC2 scaling.
- Auto Scaling Group configuration with dynamic scaling policies.
- Lambda automation for instance lifecycle and monitoring.
- TCP health checks for robust availability.
- Rich documentation, code, and AWS Console screenshots included.

---

## Project Outputs

| CloudFormation | EC2 Instances   | AMI Creation         | Health Check   | Lambda/Events    |
|---------------|-----------------|----------------------|---------------|------------------|
| ![Stack](architecture/screenshots/Screenshot-2024-06-06-212604.jpg) | ![EC2](architecture/screenshots/ec2-instances_1.jpg) | ![AMI](architecture/screenshots/Screenshot-2024-06-06-212715.jpg) | ![Health](architecture/screenshots/new-health-check.jpg) | ![Lambda](architecture/screenshots/auto-start-and-stop-EC2.jpg) |

---

## Quick Start

See [docs/project-setup-guide.md](docs/project-setup-guide.md) for full steps and commentary.

---

## Technologies Used

- AWS CloudFormation
- EC2, Custom AMI, Auto Scaling Groups
- Lambda (Python)
- EventBridge
- AWS Health Checks
- AWS Console

---

## Folder Structure

/architecture/screenshots/ # All AWS output screenshots
/docs/project-setup-guide.md # Stepwise deployment guide
/lambda/lambda_function.py

## License

For evaluation and demonstration purposes.
