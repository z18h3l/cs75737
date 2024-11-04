java c
Assignment #2
CSCI 201 Fall 2024
6% of course grade
Title
Networked Crossword Puzzle
Topics Covered
Networking
Multi-Threading
Concurrency Issues
IntroductionThis assignment will require you to create two different programs – a server and a client. You will implement a networked crossword puzzle game where multiple users can play against each other.  There will be some concurrency issues since you will need to make sure only certain clients can play at different times.  There will be many ways to design out the program, so I recommend you spend some time designing it out on paper before you begin coding.
Crossword puzzle gameA crossword puzzle is a word game consisting of interlocking horizontal and vertical grids. Participants enter letters into these grids based on the provided clues. The game concludes when all empty grids are correctly filled. Typically, the horizontal grids are referred to as ACROSS entries, while the vertical grids are called DOWN entries. Each across or down entry, which consists of a series of grids, corresponds to a word or phrase. All entries are indexed, and the corresponding clues are provided for the participants. Additionally, the intersection of an across entry and a down entry indicates a shared letter at that  specific location, which usually serves as an extra hint besides the entry-specific clues.Crossword puzzle game example
Server FunctionalityThe server will be responsible for reading the puzzle data from a file and distributing it to the clients when they  connect.  To  simplify  execution  of your  program,  only  one  game  can  be occurring at a time. Each game will have between 1 and 3 players.  The first client to connect to the server will specify the number of players.The server will have (possibly) multiple puzzle data files in a directory called gamedata at the top of your project directory. Make sure that you specify a relative path when reading the puzzle data files so the graders can drop their puzzle data files in the gamedata directory and run your program without having to change any hard-coded variables. Your server program will need to open the directory that contains all the puzzle data files and determine how many files are in that directory. It will then randomly choose one of the files to use.  Of course, if there is only one file in the directory, that is the one that will be chosen.  The names of the files are irrelevant since the server will choose one of them randomly, so they could be named anything at all.Each game data file will contain all the information needed to play a game.  The file will be formatted as a Comma Separated Value (CSV) file with ACROSS on the first line, followed by one or more lines formatted as:
ACROSS,,
,,
The file will then have a line with the word DOWN, then one or more lines formatted the same as above. Here is a sample file:
ACROSS,,
1,trojans,What is USC’s mascot?
2,dodgers,What professional baseball team is closest to USC?
3,csci,What is the four-letter prefix for Computer Science?
DOWN,,
1,traveler,What  is  the  name  of  USC’s  white  horse?
4,gold,What is one of USC’s colors?
5,marshall,Who is USC’s School of Business named after?
Your server program will need to determine if the file is formatted properly.  The only rules that you need to check are the following:
1.   The words ACROSS and DOWN exist on their own lines and only one time in the file and are followed by empty CSV fields.
2.   The other lines in the file are formatted with three parameters – integer, answer, question. a.   The three parameters are separated by commas as it is normal for CSV files.
3.   If a number in the ACROSS section matches a number in the DOWN section, the answers must start with the same letter. In the example above, the lines with trojans and traveler both start with same letter “ t”, since their number is the same, “ 1”.
4.   An answer cannot contain any whitespace.
5.   Questions normally will contain whitespace.
The server will need to determine how to format the answers in the game board so it can send out that information to all the clients (since the clients all need to render the game board the same).
There will be different ways that the data can be rendered.  You can assume that there is at least one viable way to render the game board.  In  other  words, you can assume that each word contains at least one letter that is in another word and the board can be rendered for the player. This may be challenging to figure out, so make sure to write out an algorithm to solve this before you begin coding.If the ACROSS number and the DOWN number are the same, that means the answers will start with the same letter, as explained above.  The numbers do not have to be in order in the file, but all the questions for a section will immediately follow the lines with ACROSS or DOWN. There will not be more than one ACROSS or DOWN section in a file.
All  the  answers  to  the  questions  are  case-insensitive.  The   server  will  wait  to  randomly determine a game file to read until the first client has connected.The server will listen on port 3456. When a client connects to it, it will randomly determine a game data file to read, verify that the file is formatted properly, and figure out a rendering of the game board.  If there  are no valid game files, the server should report that to the client and continue waiting for another client to connect.  At that point, it will read the directory again and randomly choose another game data file.When the first client connects to it, the first message it sends will be a number between 1 and 3 to determine how many players will be part of the game.  If the number is 1, the server can start the game. If the number is 2 or 3, the server will need to wait until the proper number of clients have connected to the server before starting the game.
Game PlayOnce the proper number of players have joined the game, the server will send out the game board to all the players.  The players will begin play in the order they joined the game.  Only one player should be able to play at a time.  More information on how the client will behave is included in the  relevant  section  below.   Every  play  should  be  sent  to  the  server.   The  server  validates whether the answer provided for 代 写CSCI 201 Fall 2024 Assignment #2
代做程序编程语言the appropriate question is correct.  If it is, the same player who guessed the correct answer will get to play again.  If it is incorrect, play will proceed to the next player. Regardless, the server should send a notification to all the clients letting them know what question the player was attempting to answer, what the player guessed, and whether it was correct.The server should keep track of how many correct answers each player has guessed, and which questions are remaining. Once all the questions have been answered correctly, the server will notify all the players of the final score (1 point for each question answered correctly) and who the winner is (or a tie, if that is the case).You do not need to maintain statistics, and there are no user accounts. Once a game concludes, the clients should terminate. The server should loop and allow a new game to begin by having another client connect. The server cannot allow another game to begin while a game is being played – only one game is played at a time.  The  server  should  never have to be restarted, though, to play subsequent games. Once the server has started, the only way to stop playing games is to “kill” the server, which is done by clicking “Terminate”, the red square, in Eclipse.
Client FunctionalityThe client will begin by welcoming the user to the game and prompting them for the server hostname and port.  If a valid connection is not made, an error message should be displayed, and the client should loop back to allow the user to enter another server hostname and port. If a KL connection is made, the first client should prompt  for the number of players.  The valid number of players is between  1  and 3.  The first client should continue prompting for the number of players until a valid number is entered.If the necessary number of players have not yet joined the game, all the clients, except for the  last one to join,  should display  a message that  says “Waiting for player x.”  The  “x” should be replaced by the player number for which we are waiting. When a player joins, all  clients, except for the last one, should display a message stating from which IP address the player  joined.Once all the required players have joined, all clients should display a message that says, “ The game is beginning.” The  server will  send the game board, which includes the crossword puzzle and the questions.Play will proceed through the players based on the order they joined the game.  Player 1 will go first and select to guess the answer to a question that is either “down” or “across”, as in “Would you like to answer a question across (a) or down (d)?”.  If “d”  or  “a”  is  not entered, the client should prompt the user again.  The user will then be prompted to enter a number for which question to answer, as in “Which number?”.  If the value  entered is not valid, the client should prompt the user again.
Once the user has entered a valid question, (let’s assume “a” and “1” were entered), the client should prompt the user to enter the answer to the question, “What is your guess for 1 across?”  The answer should be sent to the server, which will return whether it was correct or incorrect  to  all  the  connected  clients.  The  (possibly)  updated  game  board  and  remaining questions to answer will be sent to all clients. Do not display questions that have already been answered correctly.

If a player answers a question correctly, they will get to answer another question. If the answer was incorrect, play will move to the next client who joined.  If the last player that joined answers a question incorrectly, play will proceed with the first player.Play continues until all the correct answers have been guessed.  The completed game board with all the questions will be displayed, and the final scores will be shown. The final score is only based on how many questions a user guessed correctly. If one player has more correct answers than any the others, that player is declared the winner. If two players have the same score, the game is declared as a tie.
All the clients will then terminate, but the server will loop back to allow new players to join and start a new game.The sample execution that follows shows all the messages that should be displayed throughout the game.  Your  output  should be  as  close as possible to the one  shown below, though the crossword puzzle may look different for the same answers since that will be based on how your program generates it.
Grading Criteria
The way you go about implementing the solution is not specifically graded, but the output must match exactly what you see in the execution above. The maximum number of points earned is 5.
Networking (0.5)
0.1 - first client can connect to server
0.2 - only the number of clients as specified by first client can connect to server
0.2 - server allows a new game to be played after previous one ends without restarting
File Reading (0.5)
0.1 - random file chosen from gamedata directory0.1 - gamedata directory is at the top of the project folder 0.1 - server chooses game data file after first client connects 0.2 - client is notified if no valid game data file exists
Game Board Rendering (1.5)
1.5 - game board is rendered in a correct manner where there are no disjoint words (i.e., they are all connected to each other)
Game Play (1.5)
0.1 - only one player can answer a question at a time
0.1 - once a question has been answered correctly, that question is no longer displayed
0.1 - answers are case-insensitive
0.1 - proper error checking is in place for “a” and “d”0.1 - proper error checking is in place for the question number to answer0.2 - all clients are notified of answers that are guessed by other players0.1 - updated game board is displayed properly
0.3 - final score is displayed after all answers have been guessed correctly
0.1 - client terminates after the final score is displayed
0.3 - the game is played as expected with no exceptions, crashing, deadlock, starvation, or freezing
Output (0.4)
0.4 - the output is the same as what is provided in the sample above
Synchronization (0.6)
0.3 - synchronization through monitors, locks, or semaphores is used somewhere in the server
0.3 - synchronization through monitors, locks, or semaphores is used somewhere in the client



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
