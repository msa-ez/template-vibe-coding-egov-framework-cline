You must strictly create the package structure and files according to the structure below, and in cases where a '/' is included, if the file extension is specified, it should be created as a file.

[service]/    # Created by metadata name.
    src/
    └── main/
        └── java/
            └── vibecoding/
                ├── domain/
                │   └── AggregateRoot.java           # Domain entity implementing DDD Aggregate Root
                │   └── Entity.java                  # Entity classes
                │   └── Vo.java                      # VO class forming relationship with Aggregate Root Entity
                │   └── Event.java                   # Event Publish class for the results of domain state changes in the Aggregate Root
                │   └── Command.java                 # Command objects
                │   └── Repository.java              # Repository interfaces
                │   └── Enum.java                    # Enum class forming relationship with Aggregate Root Entity
                ├── infra/    
                │   └── Controller.java              # Controller providing REST API endpoints
                │   └── PolicyHandler.java           # Inbound adapter receiving events via Kafka
                │   └── EventHandler.java            # Outbound adapter publishing events via Kafka
                ├── service/
                │   └── Service.java                 # Service interface
                │   └── impl/
                │       └── ServiceImpl.java         # Service implementation
                ├── dao/
                │   └── DAO.java 
                │   └── impl/
                │       └── DAOImpl.java             # MyBatis example
                └── Application.java                 # Java Main Class
        └── resources/
            ├── log4j2.xml  [optional]
            ├── application.yml
            └── egovframework/
                ├── mapper/                     # MyBatis mapper XML files
                ├── spring/                     # Spring configuration files
                    └── context-common.xml      # Spring context configuration [optional]
                ├── message/
                    └── message-common.properties      # Message resources [optional]
└── pom.xml