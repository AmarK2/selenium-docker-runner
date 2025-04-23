pipeline{
    agent any
    stages{
        stage('Start Grid'){
            steps{
                sh 'docker-compose -f grid.yml up -d'
            }
        }
        stage('Run Test'){
            steps{
                sh 'docker-compose -f test-suites.yml up'
            }
        }
        
    }
    post{
        always{
            sh 'docker-compose -f grid.yml down'
            sh 'docker-compose -f test-suites.yml down'
        }
    }
    
}