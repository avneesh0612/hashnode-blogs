## How to add labels from a GitHub repo to another 🤔

If you maintain many open source projects, then you most likely use many custom labels across the repositories. Doing this manually will take a lot of time, so today I am going to show you how to automate this Javascript 🤩 

### Getting all the labels from a repository
To get all the labels to go to the labels in issues of the repo from where you want to get all the labels. For example  [this](https://github.com/avneesh0612/chatcube/labels) 

When you go here, you will be able to see various labels-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633247018468/-CKGyneznz.png)


The magical code needed to export all the labels-
![](https://media.giphy.com/media/3o84U6421OOWegpQhq/giphy.gif)

Open the console

- Right-click
- Inspect
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633249255829/UWNPkuAwj.png)
- Click on the console tab

Inside the console paste this code-

```


function exportGitHubLabels() {
  var labels = [];
  [].slice
    .call(document.querySelectorAll(".js-label-link"))
    .forEach(function (element) {
      labels.push({
        name: element.textContent.trim(),
        description: element.getAttribute("title"),
        // using style.backgroundColor might returns "rgb(...)"
        color:
          parseInt(
            element
              .getAttribute("style")
              .match(/--label-r:\d+;/, "")[0]
              .replace("--label-r:", "")
              .replace(";", "")
          )
            .toString(16)
            .padStart(2, "0") +
          parseInt(
            element
              .getAttribute("style")
              .match(/--label-g:\d+;/, "")[0]
              .replace("--label-g:", "")
              .replace(";", "")
          )
            .toString(16)
            .padStart(2, "0") +
          parseInt(
            element
              .getAttribute("style")
              .match(/--label-b:\d+;/, "")[0]
              .replace("--label-b:", "")
              .replace(";", "")
          )
            .toString(16)
            .padStart(2, "0"),
      });
    });
  return labels;
}

function saveDataAsJSON(data, filename) {
  const blob = new Blob([JSON.stringify(data, null, 4)], { type: "text/json" });
  const a = document.createElement("a");
  a.download = filename;
  a.href = window.URL.createObjectURL(blob);
  a.dataset.downloadurl = ["text/json", a.download, a.href].join(":");
  a.click();
}

saveDataAsJSON(exportGitHubLabels(), document.title + ".json");
```

This will take all the labels and downloading it will give us a JSON file with all the labels. The JSON object will similar to this-

```
[
  {
    "name": "💻 aspect: code",
    "description": "Concerns the software code in the repository",
    "color": "1d76db"
  },
  {
    "name": "🤖 aspect: dx",
    "description": "Concerns developers' experience with the codebase",
    "color": "1d76db"
  },
  {
    "name": "🕹 aspect: interface",
    "description": "Concerns end-users' experience with the software",
    "color": "1d76db"
  },
  {
    "name": "📄 aspect: text",
    "description": "Concerns the textual material in the repository",
    "color": "1d76db"
  },
  {
    "name": "bug",
    "description": "Something isn't working",
    "color": "d73a4a"
  },
  {
    "name": "dependencies",
    "description": "Pull requests that update a dependency file",
    "color": "0366d6"
  },
  {
    "name": "documentation",
    "description": "Improvements or additions to documentation",
    "color": "0075ca"
  },
  {
    "name": "duplicate",
    "description": "This issue or pull request already exists",
    "color": "cfd3d7"
  },
  {
    "name": "enhancement",
    "description": "New feature or request",
    "color": "a2eeef"
  },
  {
    "name": "⭐ goal: addition",
    "description": "Addition of new feature",
    "color": "c5def5"
  },
  {
    "name": "🛠 goal: fix",
    "description": "Bug fix",
    "color": "c5def5"
  },
  {
    "name": "✨ goal: improvement",
    "description": "Improvement to an existing feature",
    "color": "c5def5"
  },
  {
    "name": "good first issue",
    "description": "Good for newcomers",
    "color": "7057ff"
  },
  {
    "name": "hacktoberfest",
    "description": null,
    "color": "e99695"
  },
  {
    "name": "help wanted",
    "description": "Extra attention is needed",
    "color": "008672"
  },
  {
    "name": "invalid",
    "description": "This doesn't seem right",
    "color": "e4e669"
  },
  {
    "name": "🟥 priority: critical",
    "description": "Must be fixed ASAP",
    "color": "b60205"
  },
  {
    "name": "🟧 priority: high",
    "description": "Stalls work on the project or its dependents",
    "color": "ff9f1c"
  },
  {
    "name": "🟩 priority: low",
    "description": "Low priority and doesn't need to be rushed",
    "color": "c7d925"
  },
  {
    "name": "🟨 priority: medium",
    "description": "Not blocking but should be fixed soon",
    "color": "ffcc00"
  },
  {
    "name": "question",
    "description": "Further information is requested",
    "color": "d876e3"
  },
  {
    "name": "🚦 status: awaiting triage",
    "description": "Has not been triaged & therefore, not ready for work",
    "color": "333333"
  },
  {
    "name": "🚧 status: blocked",
    "description": "Blocked & therefore, not ready for work",
    "color": "999999"
  },
  {
    "name": "⛔️ status: discarded",
    "description": "Will not be worked on",
    "color": "eeeeee"
  },
  {
    "name": "🙅 status: discontinued",
    "description": "Not suitable for work as repo is in maintenance",
    "color": "eeeeee"
  },
  {
    "name": "🏷 status: label work required",
    "description": "Needs proper labelling before it can be worked on",
    "color": "666666"
  },
  {
    "name": "🏁 status: ready for dev",
    "description": "Ready for work",
    "color": "cccccc"
  },
  {
    "name": "🧹 status: ticket work required",
    "description": "Needs more details before it can be worked on",
    "color": "666666"
  },
  {
    "name": "wontfix",
    "description": "This will not be worked on",
    "color": "ffffff"
  }
]
```

### Adding the labels to a repo-

To add the labels to your repository, go to the issues page and add this code to the console-

```
function createLabel(label) {
  document.querySelector(".js-new-label-name-input").value = label.name;
  document.querySelector(".js-new-label-description-input").value =
    label.description;
  document.querySelector(".js-new-label-color-input").value = "#" + label.color;
  document.querySelector(".js-details-target ~ .btn-primary").disabled = false;
  document.querySelector(".js-details-target ~ .btn-primary").click();
}

function updateLabel(label) {
  let updatedLabel = false;
  [].slice
    .call(document.querySelectorAll(".js-labels-list-item"))
    .forEach((element) => {
      if (
        element.querySelector(".js-label-link").textContent.trim() ===
        label.name
      ) {
        updatedLabel = true;
        element.querySelector(".js-edit-label").click();
        element.querySelector(".js-new-label-name-input").value = label.name;
        element.querySelector(".js-new-label-description-input").value =
          label.description;
        element.querySelector(".js-new-label-color-input").value =
          "#" + label.color;
        element.querySelector(".js-edit-label-cancel ~ .btn-primary").click();
      }
    });
  return updatedLabel;
}

function createOrUpdate(label) {
  if (!updateLabel(label)) {
    createLabel(label);
  }
}

[
  // YOUR LABELS JSON HERE
].forEach((label) => createOrUpdate(label));
```

Here, you have to replace the empty array with your labels. After you hit enter all the labels will be added 🎉



**Useful links**-

 [Learn more about Git and GitHub](https://blog.avneesh.tech/series/git-and-github)

[Connect with me](https://avneesh-links.vercel.app/)  

[Nicholas Carrigan's way](https://www.nhcarrigan.com/notes/#/transfer-labels/index)