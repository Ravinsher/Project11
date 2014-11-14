#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct card
{
	char rank[15];
	char suit[15];
	int value;
};
typedef struct card Card;

int main()
{
	Card deck[52];
	int i, top = 0, player = 0, dealer = 0;
	double cash = 1000, bet;
	char answer[4];
	Card c, dealerHoldCard;
	char ranks[13][15] = { "Ace", "King", "Queen", "Jack", "Ten", "Nine", "Eight", "Seven", "Six", "Five", "Four", "Three", "Two" };
	char suits[4][15] = { "Hearts", "Clubs", "Spades", "Diamonds" };
	int values[13] = { 11, 10, 10, 10, 10, 9, 8, 7, 6, 5, 4, 3, 2 };
	for (i = 0; i < 52; i++)
	{
		strcpy_s(deck[i].rank,15, ranks[i % 13]);
		strcpy_s(deck[i].suit,15, suits[i % 4]);
		deck[i].value = values[i % 13];
	}

	for (i = 0; i < 52; i++)
	{
		Card temp = deck[i];
		int ran = rand() % 52;
		deck[i] = deck[ran];
		deck[ran] = temp;
	}

	printf("Place your bet");
	scanf_s("%lf", &bet);
	c = deck[top++];
	printf("Your first card is %s of %s\n", c.rank, c.suit);
	player = player + c.value;
	c = deck[top++];
	printf("Dealer showing %s of %s\n", c.rank, c.suit);
	dealer = dealer + c.value;
	c = deck[top++];
	printf("Your second card os %s of %s\n", c.rank, c.suit);
	player = player + c.value;
	c = deck[top++];
	dealer = dealer + c.value;
	dealerHoldCard = c;
	printf("More cards? ");
	gets_s(answer, 4);

	while (strcmp(answer, "yes") == 0 && player <= 21)
	{
		c = deck[top++];
		printf("Your next card is %s of %s\n", c.rank, c.suit);
		player = player + c.value;
		printf("More? ");
		gets_s(answer, 4);
	}

	printf("Dealer hold card is %s of %s\n", dealerHoldCard.rank, dealerHoldCard.suit);

	while (dealer < 17)
	{
		c = deck[top++];
		dealer = dealer + c.value;
	}

	if (player>21)
	{
		printf("You're busted");
		cash = cash - bet;
		printf("Your cash is now %7.2f\n", cash);
	}

	if (dealer > 21 && player <= 21)
	{
		printf("You win!");
		cash = cash + bet;
		printf("Your cash is now %7.2f\n", cash);
	}

	if (dealer <= 21 && player <= 21 && player > dealer)
	{
		printf("You win");
		cash = cash + bet;
		printf("Your cash is now %7.2f\n", cash);
	}
	else if (dealer <= 21 && player <= 21 && dealer > player)
	{
		printf("You lost.");
		cash = cash - bet;
		printf("Your cash is now %7.2f\n", cash);

		player = 0; dealer = 0;
		printf("Do you want to play again?");
			gets_s(answer, 4);
		//............................................continues....................
			//note that player=0; dealer=0; .....the } could be before or after the else if ends
	}
	}

