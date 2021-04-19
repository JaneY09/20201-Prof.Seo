 #include <string> // c++ string class
#include <Windows.h>

// https://github.com/beomjoo90/OOP2021 , branch: 1학기

using namespace std;

int main()
{	
	int bulletPos = 0;
	const int directionToLeft = 0;
	const int directionToRight = 1;

	const int screenSize = 80;
	char screen[screenSize + 1]; // 0 .. 80 : 81

	char playerFace[20]{ "(^_^)" };
	int playerPos = 10;
	int playerPos = 50;

	int key;
	char enemyFace[20]{ "(`_#)" };
	int enemyPos = 10;

	bool isReady = true;
	int bulletPos = 0;
	int direction = directionToRight;

	int major;
	int minor;

	// game loop
	while (true) {

		if (bulletPos < 0 || bulletPos >= screenSize) break;

		memset(screen, ' ', screenSize); // clear screen

		// draw game objects
		strncpy(&screen[playerPos], playerFace, strlen(playerFace) ) ; // draw player
		strncpy(&screen[enemyPos], enemyFace, strlen(enemyFace)); // draw enemy
		if (isReady == false) {
			if (bulletPos >= 0 && bulletPos < screenSize) {
				screen[bulletPos] = '-'; // draw bullet
			}
		}
		

		// handle inputs
		if (_kbhit() ) {
			key = _getch();
			printf("\nkey %c %d\n", key, key);
			major = _getch();
			switch (major) {
			case ' ':
				if (isReady == false) break;
				isReady = false;

				if (playerPos < enemyPos) direction = directionToRight;
				else direction = directionToLeft;

				if (direction == directionToRight) bulletPos = playerPos + strlen(playerFace)-1;
				else bulletPos = playerPos;
				break;
			case 224: // arrow key, function key pressed
				minor = _getch();
				switch (minor) {
				case 75: // left
					break;
				case 77: // right
					break;
				case 72: // up
					break;
				case 80: // down
					break;
				}
				break;
			}
		}

		// update
		if (isReady == false) {  // update bullet
			direction == directionToRight ? bulletPos++ : bulletPos--; // move bullet automatically
			if ( (direction == directionToLeft && enemyPos + strlen(enemyFace)-1 == bulletPos)
				|| (direction == directionToRight && enemyPos == bulletPos) ) { // 적이 총알을 맞았을 때
				strcpy(enemyFace, "(T_T)");
				isReady = true;
			}

			if (bulletPos < 0 || bulletPos >= screenSize) {
				isReady = true;
			}
		}
		// update enemy


		screen[screenSize] = '\0';  // render screen
		printf("%s\r", screen);
