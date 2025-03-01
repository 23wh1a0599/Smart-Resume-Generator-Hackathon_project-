# Smart-Resume-Generator-Hackathon_project-
------Backend code using flask-------------
from flask import Flask, render_template, request

app = Flask(_name_)

# Route for the form page
@app.route('/')
def home():
    return render_template('html1.html')

# Route to generate and display the resume in a new page
@app.route('/generate_resume', methods=['POST'])
def generate_resume():
    data = request.form.to_dict(flat=True)

    # Get multiple entries from lists
    data['skills'] = request.form.getlist('skills')
    data['experience'] = request.form.getlist('experience')
    data['projects'] = request.form.getlist('projects')

    return render_template('result.html', data=data)

if _name_ == '_main_':
    app.run(debug=True, host='0.0.0.0', port=5004)  # Changed to port 5004


---------------Frontend using html (html1.html page for designing)---------------

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Resume Generator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(to right, #74ebd5, #acb6e5);
            padding: 20px;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
            width: 80%;
            max-width: 600px;
            margin: auto;
        }
        input, button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
        }
        button {
            background: #4CAF50;
            color: white;
            cursor: pointer;
            border: none;
        }
        .add-btn {
            background: #007BFF;
        }
    </style>
</head>
<body>
<div class="container">
    <h1>Resume Generator</h1>
    <form action="{{ url_for('generate_resume') }}" method="POST" target="_blank">

        <label>Full Name:</label>
        <input type="text" name="name" required>

        <label>Email:</label>
        <input type="email" name="email" required>

        <label>Mobile No.:</label>
        <input type="text" name="mobile" required>

        <label>LinkedIn Profile:</label>
        <input type="text" name="linkedin" required>

        <!-- Education Section -->
        <h2>Education</h2>
        <label>Schooling:</label>
        <input type="text" name="schooling">

        <label>Intermediate:</label>
        <input type="text" name="intermediate">

        <label>College:</label>
        <input type="text" name="college">

        <!-- Skills Section with "Add More" -->
        <label>Skills:</label>
        <div id="skills-container">
            <input type="text" name="skills">
        </div>
        <button type="button" class="add-btn" onclick="addField('skills-container', 'skills')">Add More Skills</button>
<!-- Experience Section with "Add More" -->
        <label>Experience:</label>
        <div id="experience-container">
            <input type="text" name="experience">
        </div>
        <button type="button" class="add-btn" onclick="addField('experience-container', 'experience')">Add More Experience</button>

        <!-- Projects Section with "Add More" -->
        <label>Projects:</label>
        <div id="projects-container">
            <input type="text" name="projects">
        </div>
        <button type="button" class="add-btn" onclick="addField('projects-container', 'projects')">Add More Projects</button>

        <button type="submit">Generate Resume</button>
    </form>
</div>

<script>
    function addField(containerId, fieldName) {
        let container = document.getElementById(containerId);
        let input = document.createElement("input");
        input.type = "text";
        input.name = fieldName;
        container.appendChild(input);
    }
</script>

</body>
</html>

------------------FrontEnd using html and CSS (result.html for displaying output purpose)------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generated Resume</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: white;
            padding: 20px;
        }
        .resume-container {
            max-width: 800px;
            margin: auto;
            padding: 20px;
            border: 1px solid #ccc;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
            text-align: left; /* Aligns content to the left */
        }
        h1 {
            text-align: center;
        }
        .download-btn {
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
 <div class="resume-container">
        <h1>{{ data.get('name', '') }}</h1>
        <p><strong>Email:</strong> {{ data.get('email', '') }} | <strong>Mobile:</strong> {{ data.get('mobile', '') }}</p>
        <p><strong>LinkedIn:</strong> <a href="{{ data.get('linkedin', '') }}" target="_blank">{{ data.get('linkedin', '') }}</a></p>

        <div>
            <h2>Education</h2>
            <p><strong>Schooling:</strong> {{ data.get('schooling', 'Not Provided') }}</p>
            <p><strong>Intermediate:</strong> {{ data.get('intermediate', 'Not Provided') }}</p>
            <p><strong>College:</strong> {{ data.get('college', 'Not Provided') }}</p>
        </div>

        <div>
            <h2>Skills</h2>
            <ul>
                {% for skill in data.get('skills', []) %}
                    <li>{{ skill }}</li>
                {% endfor %}
            </ul>
        </div>
<div>
            <h2>Experience</h2>
            <ul>
                {% for exp in data.get('experience', []) %}
                    <li>{{ exp }}</li>
                {% endfor %}
            </ul>
        </div>

        <div>
            <h2>Projects</h2>
            <ul>
                {% for project in data.get('projects', []) %}
                    <li>{{ project }}</li>
                {% endfor %}
            </ul>
        </div>

        <div class="download-btn">
            <button onclick="window.print()">Download as PDF</button>
        </div>
    </div>

</body>
</html>
