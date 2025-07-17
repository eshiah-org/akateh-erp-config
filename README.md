# Setting custom apps

## Environment Variables
This project uses a `.env` file to manage sensitive environment variables. A `.env.example` file is provided as a template.

1.  Create a new file named `.env` in the root directory of the project.
2.  Copy the contents of `.env.example` into your new `.env` file.
3.  Replace the placeholder values with your actual sensitive information.

## Add app to apps.json

## Building the image
Run the following command to build the image:
```bash
docker build \
  --platform linux/amd64 \
  --build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg=FRAPPE_BRANCH=version-15 \
  --build-arg=PYTHON_VERSION=3.11.9 \
  --build-arg=NODE_VERSION=20.19.2 \
  --tag=ghcr.io/eshiah-org/akateh-erp \
  --file=Containerfile .
```


## Adding custom app
To add a custom app, first define the app in the `apps.json` file. Next, open `docker-compose.yml` file and locate the `create-site` service, locate the line where it creates a new site and add the following to that line:
```bash
--install-app education # education is the new app you added to apps.json
```

## Running the container
Run the following command to start the container:
```bash
docker compose up -d
```

## Pushing a Locally Built Image to GitHub Container Registry

If you've built the Docker image locally and want to push it to GitHub Container Registry (GHCR), follow these steps:

### 1. Create a Personal Access Token (PAT)

You need a GitHub Personal Access Token with the `write:packages` scope. Choose to create a classic token.

*   Go to your GitHub settings: [https://github.com/settings/tokens](https://github.com/settings/tokens)
*   Click on `Generate new token` (or `Generate new token (classic)`).
*   Give your token a descriptive name (e.g., `ghcr-push`).
*   **Crucially, select the `write:packages` scope.**
*   Click `Generate token` and copy the token. Keep it safe, as you won't be able to see it again.

### 2. Log in to GitHub Container Registry

Open your terminal and run the following command. When prompted for the password, paste your PAT.

```bash
echo YOUR_PERSONAL_ACCESS_TOKEN | docker login ghcr.io -u eshiah-org --password-stdin
```

You should see "Login Succeeded".

### 3. Tag Your Local Image

Tag your locally built image with the GHCR path. This is only needed if your built command din't use the registry path. For instance, assuming you named the iamge `akateh-erp:latest`. In this case, here's the command to tag it:

```bash
docker tag akateh-erp:latest ghcr.io/eshiah-org/akateh-erp:latest
```

### 4. Push the Image

Finally, push the tagged image to GitHub Container Registry. Replace `eshiah-org` with your actual GitHub username, all in lowercase.

```bash
docker push ghcr.io/eshiah-org/akateh-erp:latest
```

