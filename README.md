# flashcard

VueJS components to add spaced-repetition based flashcard to any webpage. For a WIP demo, see: https://465eg.csb.app/

Authors can create their article in a medium that helps their readers not just read, but also REMEMBER their content.
These components make it super-easy to achieve this without requiring any servers. LocalStorage is used to store user's progress on each question.

![image](https://user-images.githubusercontent.com/51651338/80322913-91cd5600-8817-11ea-9939-6bd76d2f3df8.png)

In your webpage, you would use the PracticeSet component specifying a bunch of question-answer pairs, and it will
use localStorage to store user's recall performance in order to determine practice frequency. 

See [FlashCard.vue](https://github.com/learn-awesome/flashcard/blob/master/components/FlashCard.vue) and [PracticeSet.vue](https://github.com/learn-awesome/flashcard/blob/master/components/PracticeSet.vue) to learn more about how this works.

An earlier version is available here: https://codesandbox.io/s/flashcard-sfc-with-spaced-repetition-in-vuejs-465eg?file=/components/FlashCard.vue

This was inspired by https://quantum.country
