# Electronic Voting Machine Using Arduino UNO
An Arduino-based electronic voting machine that allows users to cast votes for candidates using push buttons. The system displays real-time vote counts on a 16x2 LCD and determines the winner upon pressing the result button.
## Features
- **Candidate Selection:** Four push buttons represent four different candidates.
- **Vote Casting:** Each button press increments the vote count for the respective candidate.
- **Result Display:** Pressing the result button displays the winner on the LCD.
- **Reset Functionality:** After displaying the result, the system resets vote counts for the next session.
## Arduino Code

```cpp
#include <LiquidCrystal.h>

// Initialize the LCD (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

// Define button pins
#define S1 7  // Candidate A
#define S2 6  // Candidate B
#define S3 5  // Candidate C
#define S4 4  // Candidate D
#define S5 3  // Result Button

// Vote counters
int vote1 = 0;
int vote2 = 0;
int vote3 = 0;
int vote4 = 0;

void setup() {
  pinMode(S1, INPUT);
  pinMode(S2, INPUT);
  pinMode(S3, INPUT);
  pinMode(S4, INPUT);
  pinMode(S5, INPUT);

  lcd.begin(16, 2);
  lcd.print(" Electronic ");
  lcd.setCursor(0, 1);
  lcd.print(" Voting Machine ");
  delay(4000);

  digitalWrite(S1, HIGH);
  digitalWrite(S2, HIGH);
  digitalWrite(S3, HIGH);
  digitalWrite(S4, HIGH);
  digitalWrite(S5, HIGH);

  lcd.clear();
  lcd.setCursor(1, 0); lcd.print("A");
  lcd.setCursor(5, 0); lcd.print("B");
  lcd.setCursor(9, 0); lcd.print("C");
  lcd.setCursor(13, 0); lcd.print("D");
}

void loop() {
  lcd.setCursor(1, 1); lcd.print(vote1);
  lcd.setCursor(5, 1); lcd.print(vote2);
  lcd.setCursor(9, 1); lcd.print(vote3);
  lcd.setCursor(13, 1); lcd.print(vote4);

  if (digitalRead(S1) == LOW) { vote1++; while (digitalRead(S1) == LOW); }
  if (digitalRead(S2) == LOW) { vote2++; while (digitalRead(S2) == LOW); }
  if (digitalRead(S3) == LOW) { vote3++; while (digitalRead(S3) == LOW); }
  if (digitalRead(S4) == LOW) { vote4++; while (digitalRead(S4) == LOW); }

  if (digitalRead(S5) == LOW) {
    int totalVotes = vote1 + vote2 + vote3 + vote4;
    lcd.clear();
    if (totalVotes > 0) {
      if (vote1 > vote2 && vote1 > vote3 && vote1 > vote4) lcd.print("A is Winner");
      else if (vote2 > vote1 && vote2 > vote3 && vote2 > vote4) lcd.print("B is Winner");
      else if (vote3 > vote1 && vote3 > vote2 && vote3 > vote4) lcd.print("C is Winner");
      else if (vote4 > vote1 && vote4 > vote2 && vote4 > vote3) lcd.print("D is Winner");
      else lcd.print("Tie or No Result");
    } else {
      lcd.print("No Voting....");
    }
    delay(3000);
    vote1 = vote2 = vote3 = vote4 = 0;
    lcd.clear();
  }
}
