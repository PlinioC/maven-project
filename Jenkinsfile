pipeline {
agent 'master'
	stages{
		stage('Compilar') {
			steps{
				node(label:'master') {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'b140618f-4a3c-476f-8649-8976150b6669', url: 'https://github.com/PlinioC/maven-project.git']]])
					sh  returnStdout: true, script:'mvn clean package'
					archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
				}
			
			}
				
		}
		stage('CheckStyle') {
		steps{
			node(label:'master') {
				sh returnStdout: true, script: 'mvn checkstyle:checkstyle'
				checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''

			}
			}
		}
		stage('Despligue') {
		steps{
			node(label:'Windows') {
				copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'PipelineDemo2', selector: specific('$BUILD_NUMBER'), target: '$TOMCAT_HOME'
			}
			}
		}
	}
}