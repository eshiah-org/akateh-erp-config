# Setting up ERPNext with custom apps

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
  --build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \
  --build-arg=FRAPPE_BRANCH=version-15 \
  --build-arg=PYTHON_VERSION=3.11.9 \
  --build-arg=NODE_VERSION=20.19.2 \
  --tag=akateh-erp \
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
