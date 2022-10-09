pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                git 'https://github.com/raju2128/maven.git'
            }    
        }
        stage('ContinuousBuild')
        {
            steps
            {
                sh 'mvn package'
            }
        }
        stage('DOcker_Image')
        {
            steps
            {
                sh 'ssh ubuntu@172.31.27.163 rm webapp.war'
		sh 'ssh ubuntu@172.31.27.163 sudo docker rm -f t1'
		sh 'ssh ubuntu@172.31.27.163 sudo docker system prune -af'
                sh 'cp /home/ubuntu/.jenkins/workspace/new-deploy/webapp/target/*.war /home/ubuntu/'
                sh 'scp /home/ubuntu/webapp.war ubuntu@172.31.27.163:/home/ubuntu/'
                sh 'ssh ubuntu@172.31.27.163 sudo docker build --no-cache -t my_project:latest .'
                sh 'ssh ubuntu@172.31.27.163 sudo docker tag my_project:latest raju2128/my_project:latest'
                sh 'ssh ubuntu@172.31.27.163 sudo docker push raju2128/my_project:latest'
                sh 'ssh ubuntu@172.31.27.163 sudo docker system prune -af'
		sh 'ssh ubuntu@172.31.27.163 sudo docker run --name t1 -p 9090:8080 -d raju2128/my_project:latest
            }
        }
        stage('Continuous_delvery')
        {
            steps
            {
                sh 'ssh ec2-user@172.31.27.189 kubectl apply -f newk8s.yml'
                sh 'ssh ec2-user@172.31.27.189 kubectl rollout restart deployment.apps/my-deploy'
            }
        }
    }
}
