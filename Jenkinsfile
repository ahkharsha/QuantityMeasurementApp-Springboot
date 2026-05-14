node {
    def EC2_B_IP = "172.31.12.150"
    def DEST_DIR = "/opt/QuantityMeasurementApp-Springboot"
    def SERVICE_NAME = "quantity-measurement"

    stage('Checkout Code') {
        echo "Cloning the repository..."
        git branch: 'cloud-deployment', url: 'https://github.com/ahkharsha/QuantityMeasurementApp-Springboot.git'
    }
    
    stage('Maven Build') {
        echo "Building the Spring Boot application..."
        sh 'mvn clean package -DskipTests'
    }
    
    stage('Deploy to Backend') {
        echo "Initiating secure deployment to Backend Instance..."
        sh """
            set -e
            echo "Copying JAR to EC2 B..."
            scp -o StrictHostKeyChecking=no target/*.jar ubuntu@${EC2_B_IP}:/home/ubuntu/new-app.jar
            
            echo "Deploying and Restarting Service on EC2 B..."
            ssh -o StrictHostKeyChecking=no ubuntu@${EC2_B_IP} << EOF
                sudo systemctl stop ${SERVICE_NAME}
                
                sudo cp /home/ubuntu/new-app.jar ${DEST_DIR}/app-1.0.0.jar
                sudo chown springboot:springboot ${DEST_DIR}/app-1.0.0.jar
                sudo chmod 500 ${DEST_DIR}/app-1.0.0.jar
                rm /home/ubuntu/new-app.jar
                
                sudo systemctl start ${SERVICE_NAME}
EOF
        """
    }
}
