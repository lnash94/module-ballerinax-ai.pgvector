## Overview

Pgvector is an open-source vector similarity search extension for PostgreSQL. The pgvector connector provides specialized support for vector operations within PostgreSQL, enabling the storage and search of high-dimensional vectors directly in the database.

### Key Features

- Specialized support for vector data types in PostgreSQL
- Efficient vector similarity search (L2 distance, inner product, cosine distance)
- Seamless integration with existing PostgreSQL databases
- Support for IVFFlat and HNSW indexing
- High-performance vector storage and retrieval
- GraalVM compatible for native image builds



## Setup guide

You need a running PostgreSQL instance with the `pgvector` extension enabled. For that you can use the official pgvector Docker image.

```docker
docker run --name pgvector-db \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_DB=vector_db \
  -p 5432:5432 \
  -d pgvector/pgvector:pg17
```

To enable the pgvector extension, connect to the database and execute the following query.

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

## Quick Start

To use the pgvector vector store in your Ballerina project, modify the `.bal` file as follows.

### Step 1: Import the module

```ballerina
import ballerina/ai;
import ballerinax/ai.pgvector;
```

### Step 2: Initialize the Pgvector vector store

```ballerina
ai:VectorStore vectorStore = check new(
   host,
   user,
   password,
   database,
   tableName,
   configs = {
      vectorDimension: 1536
   }
);
```

### Step 3: Invoke the operations

```ballerina
ai:Error? result = vectorStore.add(
   [
      {
         id: "1",
         embedding: [1.0, 2.0, 3.0],
         chunk: {
               'type: "text", 
               content: "This is a chunk"
         }
      }
   ]
);

ai:VectorMatch[]|ai:Error matches = vectorStore.query({
   embedding: [1.0, 2.0, 3.0],
   filters: {
      // optional metadata filters
   }
});
```
