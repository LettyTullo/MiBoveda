trabajo del receptor es identificar donde empieza y termina la trama 
bytes de bandera
se agrega al inicio un byte que trabaja como bandera e identifica donde empieza y donde termina (lee caracteres individuales) si hay un caracter que tiene el mismo valor de la bandera, eso indica que la trama es hasta ahi 
bytes de bandera con bits
el receptor lee la cantidad de 1s hasta encontrar 5 y despues le agrega un 0