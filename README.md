# CST8915 Lab 3: Deploying the Algonquin Pet Store on Azure PaaS

**Student Name**: Randa Omer

**Student ID**: 041079985

**Course**: CST8915 Full-stack Cloud-native Development

**Semester**: Fall 2026

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/xPyLl_o4KMs)

---

## Service Repositories

- Order Service: https://github.com/RandaOmer92/order-service
- Product Service: https://github.com/RandaOmer92/product-service
- Store Front: https://github.com/RandaOmer92/store-front

---
## Deployment

| Component | Azure Service | Runtime |
|---|---|---|
| Store Front | Azure Static Web Apps | Vue.js |
| Order Service | Azure Web App Service | Node.js 24 |
| Product Service | Azure Web App Service | Python 3.13 (Flask) |
| RabbitMQ | Azure Virtual Machine | Ubuntu 24.04 |

---

## Reflection Questions

### 1. What challenges did you encounter when configuring environment variables in the GitHub Actions workflow?

The first build of the store-front did not have the backend URLs, so the products did not load. I had to add an `env:` block to the workflow file with the order-service and product-service URLs. I had to be careful with the indentation, because `env:` must line up with `runs-on:` and `name:` or the workflow breaks. I also learned that I should not put a `/` at the end of the URLs, because the code already adds `/products` and `/orders`, and a double slash would break the requests. Since Vue puts these values into the code at build time, I had to commit the change so GitHub Actions would rebuild the app.

### 2. How does deploying microservices on Azure Web App Service differ from running them locally?

When I run a service locally, I install Node.js or Python myself, create a `.env` file, and start the app with a command. On Azure Web App Service, I do not manage a server. Azure provides the runtime, gives the app a public HTTPS URL, and sets the port automatically. My code is deployed by GitHub Actions every time I push to GitHub. Settings like the RabbitMQ connection string go in the Azure App Settings instead of a `.env` file. On the free plan, the app can also take a little time to start the first time it is used.

### 3. Why is it important to use environment variables for configurations in a cloud environment?

Environment variables let the same code run in different places, like my laptop, a VM, or Azure, just by changing the settings. In this lab, the RabbitMQ connection string is stored in Azure App Settings, so the password is not in my code or on GitHub. The store-front URLs are set in the workflow, so if a backend URL changes, I only update the setting, not the code. It also lets Azure give the app its own port without me changing anything.

---

## Challenges and Learnings

- For the product-service rewrite, I used the Python (Flask) version provided by the professor and replaced the Rust files in my product-service repository, keeping the same API and products.
- GitHub deployment could not be enabled while creating the Web Apps on the Free F1 plan, so I connected GitHub afterwards using the Deployment Center.
- I did not set `PORT` manually on Azure, because App Service provides it and the code already reads it from the environment.

