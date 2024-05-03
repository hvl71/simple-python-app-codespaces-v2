# simple-python-app-codespaces

This repo contain demo code and configuration to illustrate how github codespaces can be used to launch a default dev environment in the cloud. How to build a simple python app, containerize it and test it manually.

Lot's of room for improvement. That is intentional to keep things simple. In later demos I plan on gradually increasing the fanciness factor, while still keeping things simple. I'm a strong believer in simplicity whenever possible!

The below example uses a default codespace devcontainer image somehow. So no devcontainer configured explicitly. I assume this means it cannot be used as a devcontainer locally. 

### 1. Open source via Codespaces

On the usual github code button press arrow down and select codespaces. The main branch from this repo is opened in a codespace.

### 2. Manually install Flash and create simple python app

Once in Codespaces, you'll start by setting up a simple Flask application:

- Open the terminal in Codespaces.
- Install Flask:
  ```bash
  pip install Flask
  ```
- Create a new file called `app.py` and add the following Python code:
  ```python
  from flask import Flask
  app = Flask(__name__)

  @app.route('/')
  def hello_world():
      return 'Hello, World!'

  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=8080)
  ```

### 3. Create the Dockerfile

Next, create a Dockerfile in the root directory:

- Create a file named `Dockerfile` with no extension.
- Add the following content to the Dockerfile:
  ```Dockerfile
  # Use an official Python runtime as a parent image
  FROM python:3.9-slim

  # Set the working directory in the container
  WORKDIR /app

  # Copy the current directory contents into the container at /app
  COPY . /app

  # Install any needed packages specified in requirements.txt
  RUN pip install Flask

  # Make port 8080 available to the world outside this container
  EXPOSE 8080

  # Define environment variable
  ENV NAME World

  # Run app.py when the container launches
  CMD ["python", "app.py"]
  ```

### 4. Build the Docker Image

In the Codespaces terminal, run the following command to build the Docker image:

```bash
docker build -t simple-flask-app .
```

### 5. Run the Docker Container

After the image is built, you can run it with:

```bash
docker run -p 4000:8080 simple-flask-app
```

This command runs the Docker container and maps port 8080 inside the container to port 4000 on your machine.

### 6. Verify the Application

You should now be able to access your application by navigating to `http://localhost:4000` in your web browser.

### 7. Commit and Push Changes

Finally, don't forget to commit your changes and push them to GitHub:

```bash
git add .
git commit -m "Add simple Flask app with Dockerfile"
git push
```

This sets up your Python web application with a Dockerfile in GitHub Codespaces. You can now develop and build your Docker containers entirely in the cloud!

### 8. Next steps

Mature solution a bit. We should not have to install Flask manually. We can probably also improve and automate on the build side.. 
