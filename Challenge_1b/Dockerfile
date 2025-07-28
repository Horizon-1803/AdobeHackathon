# Step 1: Use a specific Python version on a slim Linux base.
FROM --platform=linux/amd64 python:3.11-slim

# Step 2: Set a working directory inside the container.
WORKDIR /app

# Step 3: Copy only the requirements file first.

COPY requirements.txt .

# Step 4: Install Python dependencies from requirements.txt.

RUN pip install --no-cache-dir -r requirements.txt

# Step 5: Copy your entire project into the working directory.

COPY . .

# Step 6: Define the command to run your application.

CMD ["python", "-u", "run.py"]