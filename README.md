# The Scoop

![The Scoop](./.github/screenshot.jpg)

---

## Project Overview

**The Scoop** is a dynamic web application designed for article submission and community interaction. As part of Codecademy's **"Create a Back-End App with JavaScript Skill Path,"** this project challenged me to build robust routing and in-memory database logic to power a news feed experience, **with the added functionality of data persistence**.

The application, in its full implementation, enables users to:

* **Submit articles**, each containing a link and description.
* **Engage with content by upvoting and downvoting articles.**
* **Contribute to discussions by creating, editing, and deleting comments** on articles.
* **Express opinions on comments by upvoting and downvoting them.**
* **View a comprehensive feed of all articles**, as well as individual user profiles displaying their submitted articles and comments.

While the user and article management logic was provided as a foundation, my primary focus and contribution to this project involved **implementing the complete backend logic and database interactions for all comment-related functionalities, as well as integrating data persistence**.

You can see a video demonstration of the full application's capabilities (which this backend supports) here:

 [The Scoop Demo Video](./.github/TheScoopDemo.mp4)

<video width="100%" height="100%" controls>
   <source src="./.github/TheScoopDemo.mp4" type="video/mp4">
   Your browser does not support the video tag. Please click [here to watch the demo video](./.github/TheScoopDemo.mp4).
</video>

---

## Architecture & Data Management

The application's backend is built around a central `database` object (an in-memory JavaScript object) that stores all model instances, including `users`, `articles`, and **importantly, `comments`**. Each resource is stored with its ID as a key. A `nextId` property for each resource type ensures unique ID generation.

All API endpoints are defined within a `routes` object, where paths are keys and values are objects mapping HTTP verbs (GET, POST, PUT, DELETE) to their corresponding handler functions.

My work primarily extended this existing structure by integrating the `comment` data model and all related API routes. Additionally, I implemented a **data persistence layer** to ensure the database state is saved and loaded across server restarts.

### Comment Data Model (`database.comments`)

My implementation added and managed comments with the following properties:
* `id`: Unique Number for each comment.
* `body`: String content of the comment.
* `username`: String, the author's username.
* `articleId`: Number, the ID of the article the comment belongs to.
* `upvotedBy`: Array of usernames who upvoted the comment.
* `downvotedBy`: Array of usernames who downvoted the comment.
* `nextCommentId`: Number, for unique ID generation.

---

## My Contributions: Comment API Endpoints & Data Persistence

I developed the full suite of RESTful API endpoints for comments, ensuring robust data handling and proper HTTP responses, alongside the data persistence solution:

* **`POST /comments`**:
    * **Function:** `createComment`
    * **Purpose:** Creates a new comment, associates it with a user and an article, and stores it in the database. Returns the new comment with a `201 Created` status. Includes validation for missing data or non-existent users/articles (`400 Bad Request`).

* **`PUT /comments/:id`**:
    * **Function:** `updateComment`
    * **Purpose:** Updates the `body` of an existing comment identified by its ID. Returns the updated comment with a `200 OK` status. Handles cases where the comment ID or updated data is invalid (`400 Bad Request`) or the comment is not found (`404 Not Found`).

* **`DELETE /comments/:id`**:
    * **Function:** `deleteComment`
    * **Purpose:** Removes a comment from the database by its ID. Crucially, it also removes all references to this comment ID from its associated user's `commentIds` and article's `commentIds` arrays, maintaining data integrity. Returns a `204 No Content` status upon successful deletion. Handles non-existent comments (`404 Not Found`).

* **`PUT /comments/:id/upvote`**:
    * **Function:** `upvoteComment`
    * **Purpose:** Allows a specified user to upvote a comment. It adds the user's `username` to the comment's `upvotedBy` array and removes it from `downvotedBy` if present. Returns the updated comment with a `200 OK` status. Handles invalid comment IDs or usernames (`400 Bad Request`).

* **`PUT /comments/:id/downvote`**:
    * **Function:** `downvoteComment`
    * **Purpose:** Allows a specified user to downvote a comment. It adds the user's `username` to the comment's `downvotedBy` array and removes it from `upvotedBy` if present. Returns the updated comment with a `200 OK` status. Handles invalid comment IDs or usernames (`400 Bad Request`).

### Data Persistence (Bonus Implemented!)

I implemented the bonus challenge for data persistence, ensuring the `database` state is saved and loaded, allowing data to survive server restarts.

* **`loadDatabase`**: Reads the entire database state from a **YAML file** upon server startup.
* **`saveDatabase`**: Writes the current state of the `database` object to a **YAML file** after each server operation that modifies data.

This functionality was achieved using the **`Figg`** library, providing a robust solution for preserving application data.

---

## How to Set Up and Run

To explore this project locally and interact with the backend API:

1.  **Clone the Repository:** Start by cloning this repository to your local machine:
    ```bash
    git clone https://github.com/Robson16/the-scoop.git
    ```
2.  **Navigate to the Project Directory:** Change into the newly cloned project folder:
    ```bash
    cd the-scoop
    ```
3.  **Install Dependencies:** Once inside the project directory, install all necessary Node.js dependencies:
    ```bash
    npm install
    ```
4.  **Start the Server:** After dependencies are installed, start the backend server:
    ```bash
    npm run start
    # Or:
    npm run dev
    ```
    You should see confirmation in your terminal that the server is listening, likely on port `4001`. Keep this terminal window open as the server must remain active for the frontend to communicate with your backend.
    **Note:** With data persistence implemented, your data will be saved to and loaded from a YAML file, so changes will persist even if you restart the server!
5.  **Open the Application:** Open the `index.html` file in your web browser (Google Chrome is recommended for best compatibility). This HTML file provides the frontend interface that interacts with your running backend API.

---

## Testing

A comprehensive testing suite is provided with the project to validate all implemented functionalities and ensure proper handling of edge cases.

To run the tests:

1.  Ensure you've followed the "How to Set Up and Run" steps, particularly installing dependencies.
2.  In your terminal, from the project's root directory, execute:
    ```bash
    npm run test
    # Or to run tests on file changes:
    npm run test:watch
    ```
    The output will detail which tests passed or failed, providing specific feedback for debugging. Running these tests iteratively during development was crucial for confirming correct implementation and identifying issues.

---

## Technologies Used

* **JavaScript (Node.js)**: For the backend server and API logic.
* **Express.js**.
* **In-Memory Database**: A simple JavaScript object used as a temporary database.
* **Data Persistence**:
    * **YAML**: As the format for saving and loading database state.
    * **Figg**: The Node.js library used for YAML serialization/deserialization.
* **ReactJs (Frontend)**: For the client-side interface provided by Codecademy.
* **Automated Testing**:
    * [Mocha](https://mochajs.org/): A flexible JavaScript test framework.
    * [Chai](https://www.chaijs.com/): An assertion library for Node.js and the browser, often paired with Mocha.

---

## License

This project is licensed under the MIT License.
Developed by Robson16.