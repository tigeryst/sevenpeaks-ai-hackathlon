# Document summarization

## Reference Links

 * Official code examples: [link](https://github.com/Azure/azure-sdk-for-net/blob/Azure.AI.TextAnalytics_5.3.0/sdk/textanalytics/Azure.AI.TextAnalytics/samples/README.md)

 * Sample Data: [link](https://github.com/Azure-Samples/azure-search-sample-data)

----

## Lab #1

A system of summarisation should be created using simple REST API calls.

The system has a very simple flow:

 1. Receive and validate the input file, only specific text-based file (`text/plain`) could be accepted
 2. Store inside a proper storage service

    1. Blob storage is the best for the PoC
    2. File Share is the best for a container environment
    3. The file could be anonymised with a proper schema choosen by the team
    4. The file name has to be unqiue (probably use guid)

 3. Submit for summarisation

    1. In order to use Azure Cognitive Services Text Analytics, use the environment variables for URL and service Key. The team is free to use App Property and Azure Key Vault for a more secure solutions
    2. The summarisation model can be extractive or abstractive, the team is free to choose the proper one
    3. The team should provide a proper error-management business logic in order to manage the possible Exception

 4. Save the summary near the original file

    1. The original file can be moved or renamed
    2. The summary or result file can have an arbitrary but meaning-full extension, such as `.result`, while the mime type should be always equal to the input file (`text/plain`)

----

## Lab #2

Using an arbitrary input file of the previous lab exercise, the team should find a solution to understand if the document contains:

 * PII entities = Personal Identifiable Information
 * Custom restricted entities, by category or not, that are not allowed to use inside the company

Take the previous Lab exercise and evolve the point #4 and #5 in order to fulfil the requirements. The output logic could be altered based on the searches executed on the inpunt document, and, however:

 1. the summary should be created if and only if the searches have not-found result
 2. in case of searches will found something, a proper result file should be generated specified the reason and provide evidences of the result.
 3. any extensions, files or other meta-file generated should respect the mimetype of `text/plain`


