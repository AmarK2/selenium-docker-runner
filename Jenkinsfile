pipeline{
    agent any
    parameters {
        choice choices: ['firefox', 'chrome'], description: 'Select the browser', name: 'BROWSER'
        }

    stages{
        stage('Start Grid'){
            steps{
                sh "docker-compose -f grid.yml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Run Test'){
            steps{
                sh 'docker-compose -f test-suites.yml up'
                script{
                    if(fileExists('output/flight-reservation/testng-failed.xml') || fileExists('output/vendor-portal/testng-failed.xml'))
                    {
                        error('failed tests found')
                    }
                }
            }
        }
        
    }
    post{
        always{
            sh "docker-compose -f grid.yml down"
            sh "docker-compose -f test-suites.yml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
    
}