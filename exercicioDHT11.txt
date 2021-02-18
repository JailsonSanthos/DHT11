/*************************************************************************************************************************************************
                                                  EXERCÍCIO COM SENSOR DE TEMP / UMIDADE DHT11

  Material: Arduino Uno / Sensor DHT11 / Jampers
  Hardware: Ver esquema de ligação do sensor no Datasheet.
  Efeito: Leitura de temperatura e umidade ambiente

  Aluno: Jailson Correia dos Santos
  Data: 17/02/21


/************************************************************IMPORTAÇÃO DE BIBLIOTECAS***********************************************************/

#include <DHT.h>
#include <DHT_U.h>

/*************************************************************MAPEAMENTO DE HARDWARE*************************************************************/

#define dhtPin 2                         // Pino onde deve ser conectado o envio de dados do sensor.
#define dhtType DHT11                    // O tipo do sensor deve estar em maiúsculo

DHT dhtSensor (dhtPin, dhtType);         // Cria o objeto "dhtSensor" e recebe dois parâmetros. O pino de dados e o tipo do sensor.

/******************************************************************* CÓDIGO *********************************************************************/

void setup() {

  Serial.begin (9600);                    // Inicialização do monitor Serial
  dhtSensor.begin ();                     // Iniciação do objeto sensorDht

  Serial.println F("Teste de Sensor - DHT11 OK");         // Mensagem de abertura
  delay(2000);
  Serial.print F("Iniciando leitura");
  delay (1000);
  Serial.print F(".");
  delay (1000);
  Serial.print F(".");
  delay (1000);
  Serial.println F(".");
  delay (1000);

}// Fim da função Setup

void loop() {

  float umidade = dhtSensor.readHumidity();              // Chama a função da biblioteca DHT.h - recebe leitura da umidade
  float temperatura = dhtSensor.readTemperature();       // Chama a função da bibliote DHT.h  - recebe a leitura da temperatura.
  float senTermicaC = dhtSensor.convertFtoC (dhtSensor.computeHeatIndex ());    // computeHeatIndex apresenta a sensação térmica em ºF.
  // Aqui, o seu valor é convertido para ºC.

  if (isnan (umidade) || isnan (temperatura)) {          // Testa se as informações estão chegando

    Serial.println F("Falha na leitura do Sensor");

  }
  else {
    // ESCREVE O SEGUINTE TEXTO NO MONITOR SERIAL

    Serial.print F("Umidade do Ar:"); Serial.print (umidade); Serial.print F("%");
    Serial.print F(" Temp.Ambiente:"); Serial.print(temperatura); Serial.print F("ºC"); Serial.print F(" ou ");
    Serial.print (dhtSensor.convertCtoF(temperatura));    // Função da Biblioteca - Converte graus Celsius para Fahrenheit
    Serial.print F("ºF"); Serial.print F(" Sensação Térmica de: "); Serial.print(senTermicaC); Serial.println F("ºC");

    delay(10000);   // Atualiza a leitura do monitor serial a cada 2 segundos.

  }// Fim da estrutura condicional

} // Fim da função loop.
