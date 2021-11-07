## Build a quiz App with Next.js and TailwindCSS!

Hey guys, this is gonna be a tutorial plus a challenge. I also have a giveaway at the end so stay tuned ;)

### Demo

%[https://www.loom.com/share/057cf700f0a24f22ba1f3528f8f9e585]

You can try it out yourself here- https://quiz-challenge.vercel.app/

### Setting up

#### Creating a Next app with TailwindCSS

I am going to use tailwind for the basic stylings needed in the app

```
npx create-next-app next-stripe-demo -e with-tailwindcss
```

#### Cleanup

Delete everything in ***pages/index.js*** after the Head
 
```
import Head from "next/head";


export default function Home() {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <Head>
        <title>Quiz App</title>
      </Head>
    </div>
  )
}
```

#### Starting the app

```
npm run dev # npm
yarn dev # yarn
```

### Create a few questions

We are going to use the questions from a JSON array, so create a `questions.json` file in the root of the directory. The questions array should look like this-

```
[
  {
    "question": "What type of framework is Next.js?",
    "answerOptions": [
      { "answer": "Frontend" },
      { "answer": "Backend" },
      { "answer": "FullStack", "isCorrect": true },
      { "answer": "None of the above" }
    ]
  },
  {
    "question": "When was Next.js released?",
    "answerOptions": [
      { "answer": "20 September 2019" },
      { "answer": "14 January 2017" },
      { "answer": "25 October 2016", "isCorrect": true },
      { "answer": "28 March 2018" }
    ]
  },
  {
    "question": "Which CSS Framework are we using?",
    "answerOptions": [
      { "answer": "Bootstrap" },
      { "answer": "TailwindCSS", "isCorrect": true },
      { "answer": "Chakra UI" },
      { "answer": "Bulma CSS" }
    ]
  },
  {
    "question": "Which class in Tailwind is used to set flex direction of column?",
    "answerOptions": [
      { "answer": "col" },
      { "answer": "col-flex" },
      { "answer": "flex-col", "isCorrect": true },
      { "answer": "None of the above" }
    ]
  }
]
```


### Creating the UI for the Quiz
Our quiz will look like this-
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636259680363/teWazWggj.png)

**Styling the container of our app.**

I will add the following styles to the div containing the app-

```
 <div className="flex flex-col w-screen px-5 h-screen bg-[#1A1A1A] justify-center items-center">
```
   
This will give us a blank screen with the background color- #1A1A1A.


**Question section**

We are going to hard code the values for now.

```
<div className="flex flex-col items-start w-full">
  <h4 className="mt-10 text-xl text-white/60">Question 1 of 5</h4>
  <div className="mt-4 text-2xl text-white">
    What type of framework is Next.js?
  </div>
</div>
```

Now our app is looking like this

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636257025596/23Zna0jV_.png)

**Creating the answers**
We are going to map through the answers the first question, to show the options.

``` 
<div className="flex flex-col w-full">
  {questions[0].answerOptions.map((answer, index) => (
    <div
      key={index}
      className="flex items-center w-full py-4 pl-5 m-2 ml-0 space-x-2 border-2 cursor-pointer bg-white/5 border-white/10 rounded-xl"
    >
      <input type="radio" className="w-6 h-6 bg-black" />
      <p className="ml-6 text-white">{answer.answer}</p>
    </div>
  ))}
</div>
```

we also need to import questions from the questions.json file, so add this import line-

```
import questions from "../questions.json";
```

It will now give us all the options with a radio button-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636257961505/SWmhaiBfq.png)

The radio button doesn't go well with our theme, so I am going to add some custom styles for it in globals.css, so follow the instructions-

- Create a `styles` folder and `globals.css` file inside it

![Untitled.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636258196036/R40DA7RFs.png)

- Inside `globals.css` add the following-

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Import `globals.css` instead of `tailwindcss/tailwind.css` in `_app.js`

```
import "../styles/globals.css";
```

- Add the styles for the custom radio button

```
input[type="radio"]:after {
  width: 24px;
  height: 24px;
  border-radius: 24px;
  cursor: pointer;
  position: relative;
  background-color: #535353;
  content: "";
  display: inline-block;
  visibility: visible;
  border: 2px solid white;
}

input[type="radio"]:checked:after {
  width: 24px;
  height: 24px;
  border-radius: 24px;
  cursor: pointer;
  position: relative;
  background-color: #4F46E5;
  content: "";
  display: inline-block;
  visibility: visible;
  border: 2px solid white;
}
```

Now it gives us a better radio button that matches the theme like this-


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636259141168/pI1A-jI7H.png)

**Adding the buttons to navigate through the questions**

``` 
<div className="flex justify-between w-full mt-4 text-white">
  <button className="w-[49%] py-3 bg-indigo-600 rounded-lg">Previous</button>
  <button className="w-[49%] py-3 bg-indigo-600 rounded-lg">Next</button>
</div>
```

This gives us the buttons for navigating as follows.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636259680363/teWazWggj.png)


With this, we are done setting up the UI.


### Adding the logic for our quiz

**Navigation**
We are first going to build the navigation functionality

Create a state for the current question-

```
const [currentQuestion, setCurrentQuestion] = useState(0);
```

Create 2 functions to handle Next and Previous-

```
const handlePrevious = () => {
  const prevQues = currentQuestion - 1;
  prevQues >= 0 && setCurrentQuestion(prevQues);
};

const handleNext = () => {
  const nextQues = currentQuestion + 1;
  nextQues < questions.length && setCurrentQuestion(nextQues);
};
```

Assigning the functions to the respective buttons

```
  <button
    onClick={handlePrevious}
    className="w-[49%] py-3 bg-indigo-600 rounded-lg"
  >
    Previous
  </button>
  <button
    onClick={handleNext}
    className="w-[49%] py-3 bg-indigo-600 rounded-lg"
  >
    Next
  </button>
```

Remove the hardcoded values for the question-

```
<div className="flex flex-col items-start w-full">
  <h4 className="mt-10 text-xl text-white/60">
    Question {currentQuestion + 1} of {questions.length}
  </h4>
  <div className="mt-4 text-2xl text-white">
    {questions[currentQuestion].questionText}
  </div>
</div>
```

Map the answers for the current question instead of the first question-

```
questions[currentQuestion].answerOptions.map
```

Now we can easily move through the questions 🎉

%[https://www.loom.com/share/f058f666aacd457ba4c00a432b040e89]

**Ability to select options**

Create a state to hold all the selected answers-

```
const [selectedOptions, setSelectedOptions] = useState([]);
```

We will now make a function to set the selected option-

```
const handleAnswerOption = (answer) => {
  setSelectedOptions([
    (selectedOptions[currentQuestion] = { answerByUser: answer }),
  ]);
  setSelectedOptions([...selectedOptions]);
};
```

Now, we need to trigger in onClick of the option and check the radio button-

```
{questions[currentQuestion].answerOptions.map((answer, index) => (
    <div
      key={index}
      className="flex items-center w-full py-4 pl-5 m-2 ml-0 space-x-2 border-2 cursor-pointer border-white/10 rounded-xl bg-white/5"
      onClick={(e) => handleAnswerOption(answer.answer)}
    >
      <input
        type="radio"
        name={answer.answer}
        value={answer.answer}
        onChange={(e) => handleAnswerOption(answer.answer)}
        checked={
          answer.answer === selectedOptions[currentQuestion]?.answerByUser
        }
        className="w-6 h-6 bg-black"
      />
      <p className="ml-6 text-white">{answer.answer}</p>
    </div>
  ));
}
```

Now if you select an option then it will be stored as an object in the `selectedOptions` state. To check this let's console.log selectedOptions in handleAnswerOption-

```
const handleAnswerOption = (answer) => {
  setSelectedOptions([
    (selectedOptions[currentQuestion] = { answerByUser: answer }),
  ]);
  setSelectedOptions([...selectedOptions]);
  console.log(selectedOptions);
};
```

After clicking the options, it will show an array of options selected like this-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636264210489/B9jezuvP8.png)

**Calculating and showing the score**

Make 2 states, one to store the score and the other to see if we need to show the score or not-

```
const [score, setScore] = useState(0);
const [showScore, setShowScore] = useState(false);
```

Now we need to create a new function that calculates the score based on the answers-

```
const handleSubmitButton = () => {
  let newScore = 0;
  for (let i = 0; i < questions.length; i++) {
    questions[i].answerOptions.map(
      (answer) =>
        answer.isCorrect &&
        answer.answer === selectedOptions[i]?.answerByUser &&
        (newScore += 1)
    );
  }
  setScore(newScore);
  setShowScore(true);
};
```

**Show submit button instead of next on last question**

In the last question, we will need to show submit instead of next and run the `handleSubmitButton` function.

```
<button
  onClick={
    currentQuestion + 1 === questions.length ? handleSubmitButton : handleNext
  }
  className="w-[49%] py-3 bg-indigo-600 rounded-lg"
>
  {currentQuestion + 1 === questions.length ? "Submit" : "Next"}
</button>
```

Now if we submit then nothing really happens, so after we submit we should be able to see a screen like this-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636265244246/2muPcZCmA.png)

To do this we are going to render the page based on showScore's value like this-

```
{showScore ? (
    <h1 className="text-3xl font-semibold text-center text-white">
      You scored {score} out of {questions.length}
    </h1>
  ) : (
    <>
      <div className="flex flex-col items-start w-full">
        <h4 className="mt-10 text-xl text-white/60">
          Question {currentQuestion + 1} of {questions.length}
        </h4>
        <div className="mt-4 text-2xl text-white">
          {questions[currentQuestion].questionText}
        </div>
      </div>
      <div className="flex flex-col w-full">
        {questions[currentQuestion].answerOptions.map((answer, index) => (
          <div
            key={index}
            className="flex items-center w-full py-4 pl-5 m-2 ml-0 space-x-2 border-2 cursor-pointer border-white/10 rounded-xl bg-white/5"
            onClick={(e) => handleAnswerOption(answer.answer)}
          >
            <input
              type="radio"
              name={answer.answer}
              value={answer.answer}
              checked={
                answer.answer === selectedOptions[currentQuestion]?.answerByUser
              }
              onChange={(e) => handleAnswerOption(answer.answer)}
              className="w-6 h-6 bg-black"
            />
            <p className="ml-6 text-white">{answer.answer}</p>
          </div>
        ))}
      </div>
      <div className="flex justify-between w-full mt-4 text-white">
        <button
          onClick={handlePrevious}
          className="w-[49%] py-3 bg-indigo-600 rounded-lg"
        >
          Previous
        </button>
        <button
          onClick={
            currentQuestion + 1 === questions.length
              ? handleSubmitButton
              : handleNext
          }
          className="w-[49%] py-3 bg-indigo-600 rounded-lg"
        >
          {currentQuestion + 1 === questions.length ? "Submit" : "Next"}
        </button>
      </div>
    </>
  );
}
```

Now our app is working completely fine 🥳

%[https://www.loom.com/share/057cf700f0a24f22ba1f3528f8f9e585]


### Giveaway 

The winner gets the  [React and ServerLess Course](https://www.jamesqquick.com/courses/react-and-serverless-fullstack-developmnent)  course by @[James Q Quick](@jamesqquick)

To participate in this giveaway

- Make this quiz app better 
- Share it on your social media with the hashtag- `next-quiz-challenge` and don't forget to tag me :)

**Important Dates**

- 18th November 2021: Submit your projects before 18th November 12 PM IST.
- 20th November 2021: The winner will be announced on my social media.


**Few features you can add-**

- Leaderboard
- Show correct and incorrect answers
- Timer
- Improve UI


### Useful links-

[GitHub repository](https://github.com/avneesh0612/quiz-app) 

[Demo](https://quiz-challenge.vercel.app/)

[All socials](https://avneesh-links.vercel.app/)