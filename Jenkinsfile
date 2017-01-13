@Library('libpipelines@master') _

hose {
    EMAIL = 'sparta'
    MODULE = 'spark-kafka'
    DEVTIMEOUT = 20
    RELEASETIMEOUT = 20
    FOSS = true
    REPOSITORY = 'spark-kafka'

    ITSERVICES = [
        ['ZOOKEEPER': [
          'image': 'confluent/zookeeper:3.4.6-cp1']
          ],
        ['KAFKA': [
          'image': 'confluent/kafka:0.10.0.0-cp1',
          'env': ['KAFKA_ZOOKEEPER_CONNECT=%%ZOOKEEPER:2181', 'KAFKA_ADVERTISED_HOST_NAME=%%OWNHOSTNAME']
        ]],
      ]
      
    ITPARAMETERS = """
      |    -Dzookeeper.hosts=%%ZOOKEEPER:2181
      |    -Dkafka.hosts=%%KAFKA:9092
      | """
      
    DEV = { config ->
            doCompile(config)
            doIT(config)
            doPackage(config)
                        
            parallel(QC: {
                doStaticAnalysis(config)
            }, DEPLOY: {
                doDeploy(config)
            }, failFast: config.FAILFAST)        
    }
}
