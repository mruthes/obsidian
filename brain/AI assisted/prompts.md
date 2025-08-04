


Source:
Related:
Tags:  ###Postmortem

###### Contexto

###### O que deu certo?

###### O que não deu certo?

###### O que contribui para ocorrência desse resultado?

###### Como eu posso fazer melhor da próxima vez

###### Algum aprendizado final?
 
###

¡ As a paralegal, in order to store and share legal documents, I want to be able to

securely upload documents.

¡ Acceptance criteria:

– Upload should support the following common file formats: PDF, DOCX, TXT

– The maximum file size should be 20MB

– When a document is uploaded it should give a progress update to the user

– Once uploaded, the document must initially be only accessible to the person

who has uploaded it

– A report of the upload and whether it was successful or not will be stored in the

auditing feature

###

You are a SQL data generator. Generate five rows of SQL for a MySQL database.

We use the * character to delimit rules:

* The table name is identified with a # sign.

* Each table column is identified with a % sign

* Each column is described in order of name, data type and data options using the | sign

* If a column data option says random, randomize data based on the suggested format and column name

We then use the #, %, and | delimiters that we set in rules to provide instructions:

MW Here are the instructions:

# rooms

% room_name | string | random

% type | string | 'single' or 'double'

% beds | integer | 1 to 6

% accessible | boolean | true or false % image | string | random url

% description | string | random max 20 characters

% features | array[string] | 'Wifi', 'TV' or 'Safe'

% roomPrice | integer | 100 to 200




Create a JSON object with random data that contains the following fields: firstname, lastname, totalprice, deposit paid. Also, include an object called booking dates that contains checkin and checkout dates.




Source: [[Software Testing with Generative AI (Mark Winteringham) (Z-Library)]]
Related:
Tags: #prompts 
