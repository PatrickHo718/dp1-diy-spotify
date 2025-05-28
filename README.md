# DIY Music Player: A Web-Based Music Streaming Platform

This project aims to create a web-based music streaming platform, similar to Spotify, built using AWS infrastructure and modern web technologies. The goal is to demonstrate proficiency in managing music data, storing metadata in relational databases, and building an API to serve music data.

---

## Key Features
- **Data Organization**: Songs and metadata are stored in a structured format based on a well-defined schema.
- **Data Ingestion**: Modern techniques are used to ingest and store data.
- **FastAPI Backend**: A custom FastAPI API serves song and genre data in real-time.
- **AWS Infrastructure**: The project leverages AWS for cloud storage, serverless functions, and database management.

---

## Project Setup

### Infrastructure Overview
The project utilizes AWS CloudFormation templates to automate the creation of resources. Here's a breakdown of the key components:

#### S3 Bucket
An S3 bucket is used to store music files (**MP3**), metadata (**JSON**), and album images (**JPG**). This bucket is configured to allow public access and serves as the storage backbone for the frontend interface.

#### Lambda Function
An AWS **Lambda** function, built with **Chalice** in Python, is triggered when a new `.json` metadata file is uploaded to the S3 bucket. This function processes the song metadata and stores it in the database.

#### Database Service
A shared **RDS** instance is used for storing song metadata. The database consists of two tables: `songs` and `genres`. The backend connects to the database to retrieve and serve song data via the API.

#### EC2 Instance
An **EC2** instance runs a **FastAPI** container that exposes the song data via API endpoints. The FastAPI container is linked to the RDS database to serve song metadata in real-time.

---

## Data Flow: Adding a Song

The following steps describe how a new song is added to the system:

1. **Upload Song Bundle**: Upload a song bundle (**MP3 file**, **metadata in JSON**, and **album image**) to the S3 bucket.
2. **Trigger Lambda**: The Lambda function is triggered when the new `.json` metadata file is uploaded.
3. **Parse Metadata**: The Lambda function extracts metadata from the JSON file and generates the S3 URIs for the MP3 and image files.
4. **Store Data**: The metadata is inserted into the `songs` table in the RDS database.
5. **API Availability**: The song metadata becomes immediately available through the `/songs` API endpoint.
6. **Update Frontend**: The web interface is updated with the new song, accessible via the S3 website URL.

---

## Database Setup

The project uses two tables in the RDS database: **songs** and **genres**. Below is the SQL schema to create the tables and seed them with initial data.

**Genres Table**
```sql
CREATE TABLE `genres` (
  `genreid` int NOT NULL,
  `genre` varchar(20) DEFAULT NULL
);
INSERT INTO `genres` (`genreid`, `genre`) VALUES
  (1, 'Rock'),
  (2, 'Indie'),
  (3, 'Pop'),
  (4, 'Hip-hop'),
  (5, 'Jazz'),
  (6, 'Country'),
  (7, 'Classical'),
  (8, 'Other');
