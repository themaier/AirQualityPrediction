**Data:**

| DateTime            | CO(GT) | PT08.S1(CO) | NMHC(GT) | C6H6(GT) | PT08.S2(NMHC) | NOx(GT) | PT08.S3(NOx) | NO2(GT) | PT08.S4(NO2) | PT08.S5(O3) | T    | RH   | AH     |
| ------------------- | ------ | ----------- | -------- | -------- | ------------- | ------- | ------------ | ------- | ------------ | ----------- | ---- | ---- | ------ |
| 2004-03-10 18:00:00 | 2.6    | 1360        | 150      | 11.9     | 1046          | 166     | 1056         | 113     | 1692         | 1268        | 13.6 | 48.9 | 0.7578 |
| 2004-03-10 19:00:00 | 2.0    | 1292        | 112      | 9.4      | 955           | 103     | 1174         | 92      | 1559         | 972         | 13.3 | 47.7 | 0.7255 |
| 2004-03-10 20:00:00 | 2.2    | 1402        | 88       | 9.0      | 939           | 131     | 1140         | 114     | 1555         | 1074        | 11.9 | 54.0 | 0.7502 |
| 2004-03-10 21:00:00 | 2.2    | 1376        | 80       | 9.2      | 948           | 172     | 1092         | 122     | 1584         | 1203        | 11.0 | 60.0 | 0.7867 |
| 2004-03-10 22:00:00 | 1.6    | 1272        | 51       | 6.5      | 836           | 131     | 1205         | 116     | 1490         | 1110        | 11.2 | 59.6 | 0.7888 |
| 2005-04-04 13:00:00 | 2.1    | 1003.0      | NaN      | 9.5      | 961.0         | 235.0   | 702.0        | 156.0   | 1041.0       | 770.0       | 28.3 | 13.5 | 0.5139 |
| 2005-04-04 14:00:00 | 2.2    | 1071.0      | NaN      | 11.9     | 1047.0        | 265.0   | 654.0        | 168.0   | 1129.0       | 816.0       | 28.5 | 13.1 | 0.5028 |

**Observations:**

- tageszeit-abhängig (spikes zwischen 7&9 und 17&20 Uhr)
- wochentag-abhängig (Sonntag am wenigste, Samstag weniger, Freitag am meisten)
- evtl. a bissl jahreszeit abhängig (Winter mehr polution)
- ein Sensor, PT08.S3(NOx) ist verkehrt herum (hohe Werte am Sonntag, niedrige Werte am Freitag)

**Sensors:**

- für drei Features gibt es jeweils zwei Sensoren, wobei die gemessen Werte schon einiges voneinander abweichen... bisher keine Ahnung warum -> evtl. Temperatur abhängig
- PT08 Werte sind ungenauer als die anderen Werte, die durch einen reference analyzer (sehr teuer und wartungsintensiv) gemessen wurden

**First try:**

- linear-regression with 14 features
- target: 'CO(GT)'
  MSE?
  R^2?

**Second try:**

- linear-regression with 14 features
- removed "-200" values
- target: 'CO(GT)'
  Mean Squared Error: 0.44032372464480835
  R^2 Score: 0.8261430005745448

**Third try:**

- linear-regression with 14 features
- make all "-200" values in "0"
- inverted 'PT08.S3(NOx)' Sensor -> because he was negative correlated
- normalized all the features -> value 0-1
- target: 'CO(GT)'
  Mean Squared Error: 0.003109411232574027
  R^2 Score: 0.8261430005745448

**Forth try:**

- replace all "-200" values with None
- then forward fill the values
- X_scaled.fillna(method='ffill', inplace=True)
  Mean Squared Error: 0.002970506226146706
  R^2 Score: 0.8206009327903534
