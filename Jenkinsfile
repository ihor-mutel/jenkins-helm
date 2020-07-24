String dockerfile = """
FROM gradle:6.3.0-jre8

RUN echo test > test.txt

CMD ["/bin/sh"]
"""

// test pipeline trigger

triggers {
    cron('H 4/* 0 0 1-5')
}

podTemplate(containers: [
        containerTemplate(name: 'kaniko', image: 'registry.gitlab.com/griffinplus/gitlab-kaniko:latest', ttyEnabled: true, command: 'cat')
],
        volumes: [
                secretVolume(secretName: 'docker-registry-kaniko', mountPath: '/kaniko/.docker')
        ]
) {

    node(POD_LABEL) {

        stage('Build docker') {
            
            checkout scm
            
            container('kaniko') {

                stage('Build docker') {

                    writeFile file: 'Dockerfile', text: dockerfile

                    sh /* BUILD AND PUSH IMAGE */ '/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=migor/test:latest'
                }
            }
        }

    }
}