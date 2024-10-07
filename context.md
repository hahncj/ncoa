- Bulk Account Retrieval
- Need to compare address before updating
- Inbound and Outbound files need to be encrypted (Voltage?)



```mermaid
C4Context
    title Address Validation System - Context Diagram
    
    Person(User, "User", "Triggers the daily account validation process")
    
    System_Boundary(cloudPlatform, "Cloud Platform") {
        System(Scheduler, "Scheduler Service", "Schedules the daily process for account validation")
        System(AccountService, "Account Microservice", "Handles account operations and interacts with the database")
        System(EventBroker, "Event Broker", "Event-driven system to coordinate communication between services")
        System(CloudStorage, "Cloud Storage", "Temporary storage for generated files to be sent to the vendor")
        System(VendorAPI, "Vendor API", "Vendor system to validate addresses by processing files")
        System(ResponseProcessor, "Response File Processor", "Processes response files and updates accounts")
        System(AuditService, "Audit Logging Service", "Logs all system actions for auditing purposes")
        System(ErrorHandlingService, "Error Handling Service", "Handles errors during the process and retries operations")
    }
    
    System_Ext(VendorFile, "Vendor File Storage", "Third-Party System", "The vendor's file storage system to upload and receive files")
    
    Rel(User, Scheduler, "Starts the account validation process")
    
    Rel(Scheduler, EventBroker, "Triggers events for processing accounts")
    Rel(EventBroker, AccountService, "Sends event to filter and retrieve account data")
    Rel(AccountService, CloudStorage, "Stores generated files for the vendor")
    
    Rel(CloudStorage, VendorAPI, "Sends account file to the vendor for validation")
    Rel(VendorAPI, VendorFile, "Receives file and returns validation results")
    
    Rel(VendorFile, ResponseProcessor, "Receives the response file from vendor")
    Rel(ResponseProcessor, AccountService, "Updates account addresses in the database")
    Rel(ResponseProcessor, AuditService, "Logs updates for auditing")
    Rel(ResponseProcessor, ErrorHandlingService, "Handles any errors encountered during the process")
    Rel(ErrorHandlingService, User, "Notifies user about errors or failures")