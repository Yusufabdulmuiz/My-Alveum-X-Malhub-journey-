Prompt Library for AI Coding Assistants (Frontend Development)

Introduction

This prompt library contains three prompts designed for frontend development tasks: UI generation, debugging, and refactoring. Each prompt was tested using an AI coding assistant (ChatGPT).

---

1. Code Generation Prompt

Prompt

Act as a senior frontend developer.

Create a responsive React application using React.js and Tailwind CSS for managing a task list.

Requirements:
- Display all tasks
- Add a new task
- Edit an existing task
- Delete a task
- Mark tasks as completed

Each task should contain:
- Task Title
- Description
- Due Date
- Completion Status

Use component-based architecture and include proper state management using React hooks.

Test Result

AI Assistant Used: ChatGPT

Outcome

The AI generated:

- React project structure
- Reusable UI components
- Task card component
- Form component for adding/editing tasks
- State management using useState
- Responsive styling with Tailwind CSS

Evaluation

 Functional UI implementation

 Followed component-based architecture

Included responsive design

---

2. Debugging Prompt

Prompt

Act as a frontend debugging expert.

Analyze the following code and identify:
1. The cause of the error
2. Why it occurs
3. The corrected version of the code
4. Best practices to prevent similar issues

Code:

function Counter() {
const [count, setCount] = React.useState(0);

return (
<div>
<button onClick={setCount(count + 1)}>
Increase
</button>
<p>{count}</p>
</div>
);
}

Test Result

AI Assistant Used: ChatGPT

Outcome

The AI identified that:

- setCount(count + 1) executes immediately during rendering.
- The onClick event requires a callback function.
- State updates should happen only after user interaction.

Corrected Code

function Counter() {
const [count, setCount] = React.useState(0);

return (
<div>
<button onClick={() => setCount(count + 1)}>
Increase
</button>

<p>{count}</p>
</div>
);
}

Evaluation

 Correctly identified event handling issue

 Provided working React solution

 Suggested frontend debugging practices

---

3. Refactoring Prompt

Prompt

Act as a senior frontend engineer.

Refactor the following React code to improve:
- Readability
- Maintainability
- Component structure
- Adherence to React best practices

Explain all changes made.

Code:

function UserList(props){
return(
<div>
{props.users.map((user,index)=>
<div key={index}>
{user.active==true&&<p>{user.name}</p>}
</div>
)}
</div>
)
}

Test Result

AI Assistant Used: ChatGPT

Refactored Code

function UserList({ users }) {
return (
<div>
{users
.filter(user => user.active)
.map(user => (
<p key={user.name}>
{user.name}
</p>
))}
</div>
);
}

Changes Made

1. Used object destructuring for props.
2. Removed unnecessary wrapper divs.
3. Replaced "== true" with direct boolean checking.
4. Filtered active users before rendering.
5. Improved readability and maintainability.

Evaluation

 Cleaner component structure

 Improved React practices

 Easier to maintain and extend

---

Conclusion

This prompt library demonstrates how AI coding assistants can support frontend development through UI generation, debugging, and refactoring. Well-structured prompts produce more responsive, maintainable, and scalable frontend applications.
