# GitHub #

### [GitHub page](https://github.com/andreymikhit)

### [Repositories](https://github.com/andreymikhit?tab=repositories)

### List of all repositories:
$("#btn_get_repos").click(function() {
    $.ajax({
        type: "GET",
        url: "https://api.github.com/users/google/repos",
        dataType: "json",
        success: function(result) {
            for(var i in result ) {
                $("#repo_list").append(
                    "<li><a href='" + result[i].html_url + "' target='_blank'>" +
                    result[i].name + "</a></li>"
                );
                console.log("i: " + i);
            }
            console.log(result);
            $("#repo_count").append("Total Repos: " + result.length);
        }
    });
});

## Homepage under construction ... 🚧 👷‍♂️ ##

```
The final element.
```
