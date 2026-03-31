# Docker Deployment Guide for GDrive Clone

This project is configured to run in Docker using Next.js **standalone mode** for optimal performance and image size.

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Local Development / Testing

The fastest way to test the Docker setup locally is via Docker Compose.

1. Create or ensure you have a `.env.local` file with the required Appwrite credentials.
2. Run the follow command to build and start the container:
   ```bash
   docker-compose up --build
   ```
3. Open [http://localhost:3000](http://localhost:3000) in your browser.

## Manual Build & Run

If you want to build the image manually without Compose:

1. **Build the image**:
   ```bash
   docker build \
     --build-arg NEXT_PUBLIC_APPWRITE_ENDPOINT=your_endpoint \
     --build-arg NEXT_PUBLIC_APPWRITE_PROJECT=your_project_id \
     --build-arg NEXT_PUBLIC_APPWRITE_PROJECT_NAME="your_project_name" \
     --build-arg NEXT_PUBLIC_APPWRITE_DATABASE=your_db_id \
     --build-arg NEXT_PUBLIC_APPWRITE_USERS_COLLECTION=your_users_col_id \
     --build-arg NEXT_PUBLIC_APPWRITE_FILES_COLLECTION=your_files_col_id \
     --build-arg NEXT_PUBLIC_APPWRITE_BUCKET=your_bucket_id \
     -t gdrive-clone:latest .
   ```

2. **Run the container**:
   ```bash
   docker run -p 3000:3000 --env NEXT_APPWRITE_KEY=your_secret_key gdrive-clone:latest
   ```

## Pushing to DockerHub

To share your image on DockerHub, follow these steps:

1. **Log in to DockerHub**:
   ```bash
   docker login
   ```

2. **Tag your image**:
   ```bash
   docker tag gdrive-clone:latest <your-dockerhub-username>/gdrive-clone:latest
   ```

3. **Push the image**:
   ```bash
   docker push <your-dockerhub-username>/gdrive-clone:latest
   ```

## Troubleshooting

### Environment Variables
- `NEXT_PUBLIC_*` variables are baked into the JavaScript bundle at **build time**. If you change these, you must rebuild the image.
- `NEXT_APPWRITE_KEY` is a secret and is used at **runtime** (Server-side only). It can be passed via the `environment` section in `docker-compose.yml` or the `--env` flag in `docker run`.
