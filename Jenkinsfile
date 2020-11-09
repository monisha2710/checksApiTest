pipeline {
    agent any
    environment{
		PATH = "C:\\WINDOWS\\SYSTEM32;C:\\Program Files\\Java\\jdk1.8.0_121\\bin"
	}
	stages{
		stage("Compile"){
			steps{
				bat 'javac hello.java'
			}
		}
		
		stage("Run"){
			steps{
				bat 'java hello'
			}
		}
	}
    post { 
        always {
        recordIssues tools: [java(), javaDoc()], aggregatingResults: 'true', id: 'java', name: 'Java'
        recordIssues tool: errorProne(), healthy: 1, unhealthy: 20
        recordIssues tools: [checkStyle(pattern: 'target/checkstyle-result.xml'),
            spotBugs(pattern: 'target/spotbugsXml.xml'),
            pmdParser(pattern: 'target/pmd.xml'),
            cpd(pattern: 'target/cpd.xml')],
            qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]]
        }
    }
}
