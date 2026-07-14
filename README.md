# Brain Rot Central

Brain Rot Central is a full-stack meme caption platform that lets users browse AI-generated captions, vote on what is funniest, upload their own images, and explore content across communities. The project combines a static front end, Supabase authentication and database access, serverless API routes, and an external caption-generation pipeline.

## Overview

The app was built as an interactive humor lab for experimenting with AI-generated meme captions and user feedback. It supports both public browsing and authenticated participation:

- Public users can browse the caption gallery and search existing content.
- Authenticated users can vote, use Quick Vote mode, upload images, generate captions, manage their own uploads, and explore community-based content.
- Columbia-authenticated users can access the uploader discovery flow and search for other uploaders.

## Core Features

### Public Experience
- Browse a shared meme caption gallery
- Search captions by text
- View uploader, community, and AI-generation metadata
- Progressive loading for large feeds

### Authenticated Experience
- Google OAuth sign-in with optional persistent session
- Upvote and downvote captions with database persistence
- Quick Vote mode with swipe/tap interactions
- Image upload flow connected to the external caption-generation API
- Upload progress tracking and generation status updates
- Account page for managing display name and owned uploads
- Delete own uploads and associated captions/votes

### Community & Discovery Features
- Create and switch between communities
- Upload directly into a selected community
- Search for uploaders by name or Columbia email
- View uploader previews and attribution metadata

## Tech Stack

- **Frontend:** HTML, CSS, vanilla JavaScript
- **Authentication & Database:** Supabase
- **Serverless Backend:** Vercel API routes (`/api`)
- **External AI Pipeline:** `https://api.almostcrackd.ai`
- **Deployment:** Vercel

## Architecture

The application uses a lightweight architecture with a single front-end entry point and a small set of backend helper routes.

### Front End
- `index.html` contains the main application UI and client-side logic.
- `styles.css` contains the responsive design system, component styles, and interaction styling.

### Serverless API Routes
- `api/public-config.js`
  - Returns the runtime Supabase public configuration from environment variables.
- `api/public-feed.js`
  - Fetches the shared caption feed and resolves image/uploader metadata.
- `api/search-feed.js`
  - Searches caption content across the shared feed.
- `api/discover-uploaders.js`
  - Builds the uploader discovery directory for Columbia-authenticated users.
- `api/stamp-uploader.js`
  - Stamps uploader identity back onto generated caption records after uploads.

### External Services
- **Supabase** is used for:
  - OAuth authentication
  - user sessions
  - caption, image, vote, and profile data
- **Caption Pipeline API** is used for:
  - presigned upload URL generation
  - image registration
  - AI caption generation

## Upload Pipeline

The image upload flow follows a 4-step pipeline:

1. Request a presigned upload URL
2. Upload the raw image bytes to the presigned URL
3. Register the uploaded image with the caption pipeline
4. Request AI-generated captions for the uploaded image

After caption generation, the client refreshes the feed and attempts to stamp uploader identity onto the generated records.

## Expected Supabase Data Model

The project expects the following main tables to exist in Supabase:

- `captions`
- `images`
- `caption_votes`
- `profiles`

It also expects audit fields added to staging tables, including:

- `created_by_user_id`
- `modified_by_user_id`
- `created_datetime_utc`
- `modified_datetime_utc`

## Environment Variables

Set these environment variables in Vercel for production and preview deployments.

### Required
- `NEXT_PUBLIC_SUPABASE_URL` or `SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` or `SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`

### Notes
- The anon/public key is loaded at runtime through `api/public-config.js`.
- The service role key must remain private and should only be used in serverless API routes.
- Do **not** commit real production secrets to source control.

## Local Development

Because the project relies on Vercel API routes and Supabase environment variables, the recommended local workflow is to run it through a local Vercel environment.

### Prerequisites
- Node.js
- Supabase project access
- Vercel environment variables configured

### Recommended Steps
1. Install dependencies:
   ```bash
   npm install
   ```
2. Add the required environment variables locally.
3. Run the project in a local Vercel-compatible environment.
4. Open the local URL and test both public and authenticated flows.

## Deployment

This project is designed to be deployed on **Vercel**.

### Deployment Checklist
- Add the required Supabase environment variables
- Ensure Google OAuth redirect URLs are configured correctly in Supabase
- Confirm that deployment protection is disabled if public access is required for grading or demos
- Verify that the service role key is present for API routes that resolve uploader metadata

## Notable UX Features

- Responsive desktop and mobile layouts
- Search-first discovery flows to reduce overload
- Upload progress indicators and generation states
- Vote confirmations and Quick Vote progress feedback
- First-time onboarding and clearer action hierarchy

## Project Goals

This project was built to practice and demonstrate:

- full-stack web application development
- authenticated data mutation
- REST API integration
- Supabase-based user and content workflows
- responsive UI/UX design
- debugging and iteration on real user flows

## Future Improvements

Potential next steps for the project include:

- stronger moderation and reporting tools
- deeper analytics for upload and vote activity
- improved admin workflows
- richer community discovery and sorting
- more robust local persistence for user preferences

## Author

Created by Musammat Aktar as part of a humor-focused interactive web application project.
