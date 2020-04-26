# flashcard

VueJS components to add spaced-repetition based flashcard to any webpage. For a WIP demo, see: https://465eg.csb.app/

Authors can create their article in a medium that helps their readers not just read, but also REMEMBER their content.
These components make it super-easy to achieve this without requiring any servers.

In your webpage, you would use the PracticeSet component specifying a bunch of question-answer pairs, and it will
use localStorage to store user's recall performance in order to determine practice frequency.

See components/FlashCard.vue and components/PracticeSet.vue to learn more about how this works.

This was inspired by https://quantum.country