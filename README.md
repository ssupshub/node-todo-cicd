Here’s a detailed breakdown of the steps required to set up and run a project using Kubernetes, Docker, and NPM on AWS EC2 instances.

### Kubernetes Setup

1. **Create EC2 Instances:**
   - Create two EC2 instances in AWS:
     - **Master Instance:** Ubuntu, Medium instance type.
     - **Worker Instance:** Ubuntu, instance type as required.

2. **Configure Kubernetes on the Master Instance:**
   - SSH into the master instance.
   - Clone the configuration repository:
     ```bash
     git clone https://github.com/ssupshub/Kubernetes-file-.git
     ```
   - Navigate to the cloned directory and set up permissions:
     ```bash
     cd Kubernetes-file-
     chmod +x master.sh common.sh
     ```
   - Execute the common setup script:
     ```bash
     ./common.sh
     ```
   - Execute the master script:
     ```bash
     ./master.sh
     ```
   - After executing `master.sh`, note the `kubeadm` token provided. 

3. **Configure Kubernetes on the Worker Instance:**
   - SSH into the worker instance.
   - Clone the same repository:
     ```bash
     git clone https://github.com/ssupshub/Kubernetes-file-.git
     ```
   - Navigate to the directory and set up permissions:
     ```bash
     cd Kubernetes-file-
     chmod +x common.sh
     ```
   - Execute the common setup script:
     ```bash
     ./common.sh
     ```
   - Join the worker node to the master using the token obtained:
     ```bash
     kubeadm join <MASTER_PUBLIC_IP>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
     ```

4. **Verify Connection:**
   - SSH back into the master instance and check the nodes:
     ```bash
     kubectl get nodes
     ```

5. **Deploy the Application:**
   - Clone the application repository:
     ```bash
     git clone https://github.com/LondheShubham153/node-todo-cicd.git
     ```
   - Navigate to the Kubernetes configuration directory:
     ```bash
     cd node-todo-cicd/k8s
     ```
   - Create the namespace:
     ```bash
     kubectl create namespace node-app
     ```
   - Verify the namespace:
     ```bash
     kubectl get namespaces
     ```
   - Deploy the application resources:
     ```bash
     kubectl apply -f deployment.yml
     kubectl apply -f service.yml
     kubectl apply -f replica-sets.yml
     kubectl apply -f pod.yml
     ```
   - Verify resource creation:
     ```bash
     kubectl get all -n node-app
     ```

6. **Access the Application:**
   - Open a web browser and navigate to the master instance's public IP with port `30003`.

### Docker Setup

1. **Launch a New EC2 Instance:**
   - Choose an Ubuntu micro instance type.
   - Set up security group to allow all traffic.

2. **Install Docker:**
   - SSH into the new instance.
   - Update package lists and install Docker:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install docker.io -y
     ```
   - Verify Docker installation:
     ```bash
     docker --version
     ```
   - Start and enable Docker:
     ```bash
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

3. **Install Docker Compose:**
   - Install Docker Compose:
     ```bash
     sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```
   - Verify Docker Compose installation:
     ```bash
     docker-compose --version
     ```

4. **Build and Run the Project:**
   - Clone the project repository:
     ```bash
     git clone https://github.com/LondheShubham153/node-todo-cicd.git
     ```
   - Navigate to the project directory:
     ```bash
     cd node-todo-cicd
     ```
   - Build and run the project using Docker Compose:
     ```bash
     docker-compose build
     docker-compose up
     ```

5. **Access the Application:**
   - Open a web browser and navigate to the instance's public IP with port `30003`.

6. **Shutdown the Project:**
   - To stop the Docker containers:
     ```bash
     docker-compose down
     ```

### NPM Setup

1. **Launch a New EC2 Instance:**
   - Choose an Ubuntu micro instance type.
   - Set up security group to allow all traffic.

2. **Install Node.js and NPM:**
   - SSH into the new instance.
   - Update package lists and install Node.js:
     ```bash
     sudo apt update && sudo apt upgrade -y
     curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
     sudo apt install -y nodejs
     ```
   - Verify Node.js and NPM installation:
     ```bash
     node -v
     npm -v
     ```

3. **Set Up the Project:**
   - Clone the project repository:
     ```bash
     git clone https://github.com/LondheShubham153/node-todo-cicd.git
     ```
   - Navigate to the project directory:
     ```bash
     cd node-todo-cicd
     ```
   - Install and update dependencies:
     ```bash
     npm install
     npm update
     npm audit fix --force
     ```
   - Start the application:
     ```bash
     npm start
     ```

4. **Access the Application:**
   - Open a web browser and navigate to the instance's public IP with port `30003`.

