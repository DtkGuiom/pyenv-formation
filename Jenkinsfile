def versionpython= ["python3.7", "python3.8", "python3.9"]

pipeline {
  agent none
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
    timeout(time: 60, unit:'MINUTES')
    timestamps()
  }
  stages {
    stage("install req"){
	  agent { label 'SlaveLinux' }
      steps{
        script{
	        versionpython.each {item ->
			    withPythonEnv("/usr/bin/${item}") {
						sh "python --version"
						sh "pip --version"
						sh "pip install -r requirements.txt"
			    }
	        }
		}
      }
    }
	stage('Compilation') {
        agent { label 'SlaveLinux' }
		  steps{
			script{
				versionpython.each {item ->
					withPythonEnv("/usr/bin/${item}") {
							sh 'python -m py_compile sources/add2vals.py sources/calc.py'
					}
				}
			}
		}
    }	
	stage('Tests') {
        agent { label 'SlaveLinux' }
		  steps{
			script{
				versionpython.each {item ->
					withPythonEnv("/usr/bin/${item}") {
							sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
							junit 'test-reports/results.xml'
					}
				}
			}
		}
    }
  }
}
