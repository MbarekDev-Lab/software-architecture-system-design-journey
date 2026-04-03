# Highly Scalable Image Sharing Platform - Part 1

## Overview

This lecture starts a real world system design case study:

Design an image sharing platform similar to Instagram, Google Photos, or Imgur, where users can upload, store, and view images at very large scale.

The main objective of this part is not to jump into specific technologies, but to follow the right system design process:

1. Clarify requirements
2. Estimate scale
3. Design a high-level architecture
4. Identify key bottlenecks early

## 1. Problem Understanding

Before discussing databases, storage, or cloud services, the first step is to understand the problem clearly.

### Functional Requirements

- Users can upload images
- Users can view image
- Users can share images
- Optional features:
  - Likes
  - Comments
  - User profiles

### Non-Functional Requirements

- Scalability: the system should support millions of users
- High availability: the platform should rarely go down
- Low latency: image uploads and image loads should be fast
- Durability: uploaded images must not be lost

### Why This Step Matters

This is one of the most important interview habits.

Many candidates jump straight into technologies and architecture diagrams. A stronger approach is to first define what the system must do and what quality constraints it must satisfy.

## 2. Back-of-the-Envelope Scale Estimation

The lecture introduces rough scale estimation to understand whether the system can fit on a single machine or requires distributed infrastructure.

### Example Assumptions

- 10 million users
- Each user uploads 1 image per day
- Average image size = 5 MB

### Daily Storage

10M users x 1 image/day x 5 MB = 50 TB/day

### Yearly Storage

50 TB/day x 365 days = 18,250 TB/year

That is approximately:

- 18 PB/year

### Main Insight

This scale is far beyond what a single server can handle reliably.

The system must use distributed storage.

## 3. High-Level Architecture

At a high level, the system can be represented like this:

```text
Client (Mobile/Web)
        |
        v
API Gateway / Load Balancer
        |
        v
Application Servers
        |
        +--> Object Storage
        |
        +--> Metadata Database
```

### Main Components

#### Client

- Mobile app or web browser
- Uploads images
- Fetches images for display

#### API Gateway / Load Balancer

- Receives incoming traffic
- Distributes requests across multiple servers
- Prevents a single server from becoming overloaded

#### Application Servers

- Handle upload requests
- Authenticate users
- Process image-related metadata
- Coordinate storage and database operations

#### Object Storage

- Stores the actual image files
- Designed for large binary objects
- Scales much better than storing raw images in a traditional database

#### Metadata Database

- Stores image metadata, not the image bytes themselves
- Typical metadata includes:
  - Image URL or storage key
  - User ID
  - Upload timestamp
  - Sharing or visibility settings

## 4. Core Design Challenge

One of the most important ideas introduced in this lecture is the separation between image data and metadata.

### Problem

- Images are large binary files
- Databases are not ideal for storing large media objects at massive scale

### Better Approach

- Store image files in object storage
- Store only metadata in the database

### Why This Matters

- Better scalability
- Better cost efficiency
- Cleaner separation of responsibilities
- Easier system evolution later

## 5. Image Upload Flow

The lecture introduces a simple upload pipeline:

```text
User uploads image
        |
        v
Application server receives request
        |
        v
Image stored in object storage
        |
        v
Metadata saved in database
        |
        v
Image URL returned to user
```

### Upload Steps

1. The user sends an upload request from a mobile or web client.
2. The application server receives and validates the request.
3. The image file is stored in the storage system.
4. The server writes image metadata to the database.
5. The system returns a URL or reference that can later be used to retrieve the image.

## 6. Key Concepts To Remember

### Separation of Concerns

- Images and metadata should be handled differently
- Storage and database systems have different responsibilities

### Horizontal Scaling

- Instead of making one server bigger, add more servers
- This is the standard approach for internet-scale systems

### Distributed Storage

- Required for petabyte-scale image storage
- Necessary when capacity and durability requirements become very large

### Structured System Design Process

- Clarify requirements first
- Estimate scale second
- Build the high-level design next
- Identify bottlenecks before optimizing details

## 7. Engineering Mindset From This Lecture

The lecture reinforces an important system design habit:

Do not start with tools.

Start with these questions instead:

- What is the expected load?
- What will break first?
- Where are the bottlenecks?
- Which components need to scale independently?

This is the kind of reasoning expected in strong design interviews and real production architecture discussions.

## 8. What Likely Comes Next

Part 2 usually goes deeper into the read path and performance optimization topics such as:

- CDN usage
- Caching strategies
- Image compression
- Thumbnail generation
- Read scaling versus write scaling

## Quick Summary

This lecture lays the foundation for designing an Instagram-like image platform.

The main takeaways are:

- Start by clarifying functional and non-functional requirements
- Use rough scale estimates to understand system constraints
- Design around distributed storage for image files
- Keep image metadata in a database
- Think in terms of bottlenecks, scaling, and separation of concerns

## Interview Takeaway

If this appears in a system design interview, a strong answer starts with:

1. Clarifying requirements
2. Estimating traffic and storage
3. Separating image storage from metadata storage
4. Presenting a simple, scalable high-level architecture
