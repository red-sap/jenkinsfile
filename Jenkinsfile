@Library("jenkinslib") _ 
def tools = new org.devops.tools()
def sonar =new org.devops.sonarqube()

String workspace = "/opt/jenkins/workspace"

String srcUrl = "${env.srcUrl}"
String branch = "${env.branch}"
String command = "${env.command}"
String tool_package = "${env.tool_package}"
//pipline
pipeline{
// 	agent {node { label "master"
// 					customWorkspace "${workspace}"
// 		}
    agent any
    parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }
    // tools {
    //     maven 'apache-maven-3.0.1' 
    // }
// 	}
	options{
		timestamps() //日志的输出有时间
		skipDefaultCheckout() //删除隐式checkout scm语句（声明式默认会去下载代码）
		disableConcurrentBuilds() //禁止并行
		timeout(time: 1,unit: "HOURS") //流水线超时时间设置为1h
	}


	stages{
        stage('step getcode') {
			steps{
			script{
            	checkout([$class: 'GitSCM', branches: [[name: "${branch}"]], extensions: [], userRemoteConfigs: [[credentialsId: 'a8cdb733-01c2-4786-9f63-338eb4c91c91', url: "${srcUrl}"]]])
				}
			}
		}
		stage('step codescan') {
			steps{
			script{
            	tools.PrintMes("代码扫描","green")
				sonar.SonarScan("${JOB_NAME}","${JOB_NAME}","src")
				}
			}
		}
        stage('step build') {
			steps{
			script{
				tools.exec(tool_package,command)
				}
			}
		}
	}
	post {
		always{
			script{
				println("always")
			}
		}
		success {
			script{
				currentBuild.description += "\n 构建成功"
			}
		}
		failure {
			script{
				currentBuild.description += "\n 构建失败"
			}
		}
		aborted {
			script{
				currentBuild.description += "\n 构建取消"
			}
		}
	}

}
