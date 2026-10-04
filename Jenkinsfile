node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        deleteDir()
    }

    stage('Clone Repo') {
        echo 'Cloning the repository...'
        checkout scm
    }

    stage('Deploy to EC2') {
        echo 'Deploying the application to EC2...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}
            npm install
            npm run build

            # Stop any process on port 3000, then run Next.js in background
            sudo fuser -k 3000/tcp || true
            nohup npm run start > /var/log/nextjs.log 2>&1 &
        """
    }
}