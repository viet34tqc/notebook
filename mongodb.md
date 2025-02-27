# MongoDB

## Key concept

- Documents: In MongoDB, data is stored as BSON documents (binary JSON). A document is similar to a `row` in a relational database but has a flexible schema.
- Collections: Groups of documents, similar to `tables` in relational databases.
- Databases: Containers for collections.
- Fields: Key-value pairs within documents.

## Use cases

MongoDB is commonly used in projects that require high scalability, flexible schema design, and fast read/write performance

1. Real-time Analytics & Logging

- MongoDB: Better for high-speed inserts and unstructured log data.
- PostgreSQL/MySQL: Can be used but may require extra indexing and partitioning for performance. PostgreSQL has better support for JSONB, making it a solid alternative.