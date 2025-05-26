# DIY Music Player: A Web-Based Music Streaming Platform

This project involves building a customized, web-based music player similar to Spotify, utilizing AWS infrastructure and modern web technologies. The objective is to demonstrate proficiency in managing music data, storing metadata in relational databases, and building an API to serve music data. The key steps involve:

- Organizing and creating data files based on a defined schema.
- Using modern techniques to ingest and store data.
- Storing song metadata in a relational database.
- Exposing this data through a FastAPI-based API.

Explore the project through the following links:
- **Frontend Example**: [Live Demo](http://nem2p-dp1-spotify.s3-website-us-east-1.amazonaws.com/)
- **API Endpoints**:
  - [Songs API](https://bv1e9klemd.execute-api.us-east-1.amazonaws.com/api/songs)
  - [Genres API](https://bv1e9klemd.execute-api.us-east-1.amazonaws.com/api/genres)

## Project Setup

### 1. Fork and Clone the Repository

Start by forking this repository to track your progress. You don’t need to submit pull requests back to the upstream; simply push your changes to your own fork.

### 2. Infrastructure Overview

The project leverages AWS CloudFormation templates to automate resource creation. Here's an overview of the key components:

#### S3 Bucket
The project creates an S3 bucket for storing music files, metadata, and images. The bucket is configured to allow public access and serve as a website for the frontend interface. You will upload song bundles (MP3, JSON metadata, JPG image) to this bucket.

#### Lambda Function
An AWS Lambda function, created using the Python framework **Chalice**, is triggered when a new `.json` metadata file is uploaded to the S3 bucket. This function processes the song metadata and stores it in the database.

#### Database Service
A shared RDS instance is used to store the song metadata. You will create tables for songs and genres and configure the database connection within the application.

#### EC2 Instance
An EC2 instance is set up to run a FastAPI container, which serves the music data via API endpoints. The FastAPI container will be linked to the RDS database to retrieve and serve song data.

### 3. Data Flow: Adding a Song

Here’s the step-by-step process when a new song is uploaded to the system:

1. **Upload Song Bundle**: A song bundle (MP3, metadata, and album image) is uploaded to the S3 bucket.
2. **Trigger Lambda**: The Lambda function is triggered by the arrival of a new `.json` metadata file.
3. **Parse Metadata**: The Lambda function extracts the metadata from the `.json` file and calculates the S3 URIs for the MP3 and image files.
4. **Store Data**: The metadata is inserted into the `songs` table in the RDS database.
5. **API Availability**: The song metadata is immediately accessible through the `/songs` API endpoint.
6. **Update Frontend**: The web interface is updated to display the new song, which can be accessed via the S3 website URL.

### 4. Database Setup

You will set up two tables in the RDS database: `songs` and `genres`. Use the SQL schema provided to create these tables, and then seed the database with an initial song entry.

Here’s the schema for the tables:

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
