---
layout: post
title: Digital transformation for the rest of us
subtitle: Each post also has a subtitle
gh-repo: digitalskills-info/digitalskills-info.github.io
gh-badge: [star, fork, follow]
tags: [org]
comments: true
---


<form action="/assets/mailform.php" method="post" id="contact-form">
  <h2>Contact us</h2>

  <?php echo((!empty($errorMessage)) ? $errorMessage : '') ?>
  <p>
    <label>First Name:</label>
    <input name="name" type="text" value="Your Name"/>
  </p>
  <p>
    <label>Email Address:</label>
    <input style="cursor: pointer;" name="email" value="name@email.com" type="text"/>
  </p>
  <p>
    <label>Message:</label>
    <textarea name="message">What interests you about this project?</textarea>
  </p>
  <p>
    <input type="submit" value="Send"/>
  </p>
</form>
<script>
    const constraints = {
        name: {
            presence: { allowEmpty: false }
        },
        email: {
            presence: { allowEmpty: false },
            email: true
        },
        message: {
            presence: { allowEmpty: true }
        }
    };

    const form = document.getElementById('contact-form');

    form.addEventListener('submit', function (event) {
        const formValues = {
            name: form.elements.name.value,
            email: form.elements.email.value,
            message: form.elements.message.value
        };

        const errors = validate(formValues, constraints);

        if (errors) {
            event.preventDefault();
            const errorMessage = Object
                .values(errors)
                .map(function (fieldValues) {
                    return fieldValues.join(', ')
                })
                .join("\n");

            alert(errorMessage);
        }
    }, false);
</script>
