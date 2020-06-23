pipeline{
    agent{
        label 'oulu-lab'
    }
    stages{
        stage('Install Vagrant plugins'){
            steps{
                sh """
                    vagrant plugin install vagrant-disksize
                    vagrant plugin install vagrant-reload
                """
            }
        }
        stage('Boot VM'){
            steps{
                sh 'cp ~/Vagrantfile1 Vagrantfile'
                sh 'vagrant up --provision'
            }
        }
        stage('Build'){
            steps{
                sh 'vagrant ssh -c "build_maestro"'
            }
        }
        stage('System test'){
            steps{
                catchError(buildResult: 'UNSTABLE', message: 'Test cases failure!!!', stageResult: 'UNSTABLE') {
                    sh """
                        cd tests
                        npm i
                        npm test
                    """
                }
            }
        }
        /*stage('Coverity'){
            steps{
                withAWS(region: "eu-west-1", credentials: "AKIA4NI2GQUKC43TCTC2") {
                    sh """
                        vagrant ssh -c "cov-configure --go"
                        vagrant ssh -c "cd /home/vagrant/work/gostuff/src/github.com/armPelionEdge/maestro; cov-build --dir coverity go build"
                        vagrant ssh -c "cd /home/vagrant/work/gostuff/src/github.com/armPelionEdge/maestro; cp -r coverity/ /vagrant/"
                        tar -zcvf coverity-maestro_master.tar.gz coverity
                        aws s3 cp coverity-maestro_master.tar.gz s3://coverity-reports/
                    """
                }
            }
        }*/
    }
}
