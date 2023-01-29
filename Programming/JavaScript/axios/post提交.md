```java
axios.post(posthost + "login", {
            user: user.value,
            passwd: passwd.value
        })
            .then(function (response) {
                localStorage.setItem("name", response.data.name)
                localStorage.setItem("user_id", response.data.user_id)
                localStorage.setItem("user_type", response.data.user_type)
                switch (response.data.user_type) {
                    case 1:
                        window.location.href = "teacher.html";
                        break;
                    case 2:
                        window.location.href = "student.html";
                        break;

                }
            })
            .catch(function (error) {
                // console.log(error);
            });
```

