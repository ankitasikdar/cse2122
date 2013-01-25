---
title: Debugging techniques
layout: default
---

> The Titanic effect: The severity with which a system fails is
> directly proportional to the intensity of the designer's belief that
> it cannot fail. -- R.A. De Millo, R.J. Lipton, and A.J. Perlis,
> "Social processes and proofs of theorems and programs,"
> *Communications of the ACM*, 1979

## Universal technique

`cout`

> A program is a human artifact; a real-life program is a complex
> human artifact; and any human artifact of sufficient size and
> complexity is imperfect. -- (*op. cit.*)

## Some Examples :
{% highlight cpp %}
#include <iostream>
using namespace std;

int main()
{
    char card('y'); // for entering the cards in the hand
    int numcards(0), score(0), numace(0); // num cards is total number of cards, score is the total score and numace is the number of aces

    while(card=='Y' || card=='y') // loop to continue scoring hands
    {
        //prompt for number of cards

        cout << "Enter the number of cards in your hand: ";
        cin >> numcards;
        // if number of cards is invalid keep prompting for a valid number
        while(!(numcards<=5 && numcards>=2)) 
        {
            cout << "Number of cards in your hand must be a number between 2 and 5." << endl;
            cout << "Enter the number of cards in your hand: ";
            cin >> numcards;
        }

        for(int i=1; i <= numcards; i++) // loop to enter in cards and score
        {
            cout << "Enter a card (2-9, t, j, k, q, a): ";
            cin >> card;

            if(card == '2')
            {
                score+=2;
            }
            else if(card=='9')
            {
                score+=9;
            }
            else if(card=='t' || card=='T' || card=='j' || card=='J' 
            || card=='q' || card=='Q' || card=='k' || card=='K')
            {
                score+=10;
            }
            else if(card=='a' || card=='A')
            {
                score+=11;
                numace++;
            }
            else
            {
                cout << "Invalid input." << endl;
            }
        }

        for(int n=1; n <= numace; n++) // loop to correctly value the aces in a hand
        {
            if(score>21)
            {
                score-=10;
            }
        }
        // if score is less than or equal to 21 print score otherwise the hand is busted
        if(score <= 21) 
        {
            cout << "Your score is " << score << "." << endl;
        }
        else
        {
            cout << "Busted!" << endl;
        }

        // prompt to score another hand

        cout << "To try again enter y or any other character to quit: ";
        cin >> card;

        }

    return 0;
}
{% endhighlight %}

## CodeBlocks specific techniques

## Visual Studio specific techniques

## Xcode specific techniques

> Programs are jagged and full of holes and caverns. Every programmer
> knows that altering a line or sometimes even a bit can utterly
> destroy a program or mutilate it in ways that we do not understand
> and cannot predict. And yet at other times fairly substantial
> changes seem to alter nothing; the folklore is filled with stories
> of pranks and acts of vandalism that frustrated the perpetrators by
> remaining forever undetected. -- (*op. cit.*)


## Linux specific techniques

> There is a classic science-fiction story about a time traveler who
> goes back to the primeval jungles to watch dinosaurs and then
> returns to find his own time altered almost beyond
> recognition. Politics, architecture, language--even the plants and
> animals seem wrong, distorted. Only when he removes his time-travel
> suit does he understand what has happened. On the heel of his boot,
> carried away from the past and therefore unable to perform its
> function in the evolution of the world, is crushed the wing of a
> butterfly. Every programmer knows the sensation: A trivial, minute
> change wreaks havoc in a massive system. Until we know more about
> programming, we had better for all practical purposes think of
> systems as composed, not of sturdy structures like algorithms and
> smaller programs, but of butterflies' wings. -- (*op. cit.*)

