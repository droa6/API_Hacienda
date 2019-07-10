# Funcionamiento del Callback URL

El parámetro del callback url se pasa cuando se envía el documento electrónico.
Por ejemplo, tomando como referencia este mismo API, se pasaría aqui:
 - https://github.com/droa6/API_Hacienda/blob/66440d44dedc39535f2e40b37435edf03137d49b/api/contrib/send/send.php#L49

Una vez procesado el documento, Hacienda hace un POST de vuelta a esa URL con los datos de procesamiento en formato JSON.

Usando como ejemplo la función en mostrada arriba en callBack.php, se puede ver como extraer los datos recibidos de Hacienda.
 - https://github.com/droa6/API_Hacienda/blob/66440d44dedc39535f2e40b37435edf03137d49b/api/contrib/callback/callBack.php#L19

Una vez recibidos es cuestión de actualizar el estado en su BD local para tener una referencia más cercana donde consultar.
El mensaje trae tambien el XML de recepción, así que puede actualizar ese valor también para usarlo según su sistema requiera.

Observaciones:
- Al menos en mi caso, no logré recibir bien los datos cuando el callback URL era un HTTPS, así que terminé usando HTTP.
- Tome en cuenta que el callback url debe estar desprotegido de cualquier autenticacion o Hacienda no va a poder completar la llamada.
- El servicio debe retornar un estado de OK (200) para dar por recibido el mensaje. En caso de no recibirlo o recibir error, Hacienda intenta hasta 3 veces (no estoy seguro en cuanto tiempo) y luego da por descartado el callback, haciendo la anotación de que el servicio de callback url provisto no respondió adecuadamente.
- Este callback funciona también en stag
- Para pruebas inicialmente puede usar algún servicio como https://webhook.site/ y así ver como la respuesta viene directamente de Hacienda antes de parsearla.
