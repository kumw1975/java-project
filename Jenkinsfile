node('linux') {
    stage('Unit Tests') {
        git 'https://github.com/kumw1975/java-project.git'
        sh 'ant -f test.xml -v'
    }
    stage('Build') {
        sh 'ant -f build.xml -v'
    }
    stage('Deploy') {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkins_git_aws_cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          sh("aws s3 cp $WORKSPACE/*/rectangle-${BUILD_NUMBER}.jar s3://seis665bucket1/HW10/rectangle-${BUILD_NUMBER}.jar")  
        }
    }
    stage('Report') {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkins_git_aws_cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          sh("aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins")  
        }
    }
}
