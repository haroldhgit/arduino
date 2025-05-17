
#include <Servo.h>  // Biblioteca do servo motor

Servo servoMotor1;  // Primeiro servo (oscila conforme o BPM)
Servo servoMotor2;  // Segundo servo (pulsação de 10 graus)
const int sensorPin = A0;  // Pino analógico do sensor de batimentos cardíacos
int bpm = 0;  // Variável para armazenar os batimentos por minuto
int pos1 = 0;  // Posição do primeiro servo
int basePos2 = 90;  // Posição base do segundo servo (posição neutra)
int intervaloPulso = 600; // Tempo base entre pulsos, ajustado pelo BPM

void setup() {
    servoMotor1.attach(9);  // Conectar o primeiro servo ao pino 9 do Arduino
    servoMotor2.attach(10); // Conectar o segundo servo ao pino 10 do Arduino
    Serial.begin(9600);  // Inicializa a comunicação serial
}

void loop() {
    int sensorValue = analogRead(sensorPin);  // Lê o valor do sensor
    bpm = map(sensorValue, 500, 900, 60, 120);  // Mapeia os valores do sensor para uma faixa de BPM

    Serial.print("Batimentos por minuto: ");
    Serial.println(bpm);

    // Primeiro servo oscila conforme o BPM
    pos1 = map(bpm, 60, 120, 0, 180);  
    servoMotor1.write(pos1);  

    // Ajusta a frequência do pulso do segundo servo baseado no BPM
    intervaloPulso = map(bpm, 60, 120, 800, 400);  // Ajusta tempo do pulso com base no BPM

    // Segundo servo pulsa 10 graus sincronizado ao BPM
    servoMotor2.write(basePos2 + 5);  // Avança 5 graus
    delay(intervaloPulso / 2); // Ajusta o tempo conforme BPM
    servoMotor2.write(basePos2 - 5);  // Retorna 5 graus
    delay(intervaloPulso / 2); // Ajusta o tempo conforme BPM
}

