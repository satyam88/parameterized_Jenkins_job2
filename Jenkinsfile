pipeline {
    agent any

    parameters {
        choice(
            name: 'AWS_REGION',
            choices: ['us-east-1', 'us-west-1', 'us-west-2', 'eu-central-1', 'eu-west-1', 'ap-south-1'], 
            description: 'Select the AWS region'
        )
    }

    environment {
        AWS_REGION = "${params.AWS_REGION}"
    }

    stages {
        stage('List AWS Resources') {
            environment {
                // Fetch AWS credentials from Jenkins securely
                AWS_ACCESS_KEY_ID = credentials('aws-credentials-id').username
                AWS_SECRET_ACCESS_KEY = credentials('aws-credentials-id').password
            }
            steps {
                script {
                    echo "Listing resources in the region: ${AWS_REGION}"

                    // List all EC2 instances
                    echo "Fetching EC2 Instances..."
                    sh """
                        aws ec2 describe-instances --region ${AWS_REGION} --query "Reservations[*].Instances[*].{InstanceId:InstanceId, State:State.Name, InstanceType:InstanceType, PublicIpAddress:PublicIpAddress}" --output table
                    """

                    // List all EBS volumes
                    echo "Fetch
