# Les tests

## Execution

cf. test/index-messages-(xhr|jsonp).html
Execution uniquement en localhost !

### Principe

L'API
- construit la requête à partir des options,
- appel le service (XHR request),
- recupère la reponse du service (XHR response)
- analyse la reponse puis la renvoie sous forme d'un JSON

L'idée est de capturer ('mocker') la requête et la reponse du service (XHR).
On laisse l'API construire la requête mais l'action 'request' du service est simulée :
    on compare donc la requête créée par l'API à celle simulée, elle doivent être identique !

Pour l'API, ce mécanisme est completement transparent, elle recupère alors une reponse simulée qu'elle va analyser puis la renvoyer
sous forme d'un JSON :
    on teste que l'analyse et le retour de l'API est bien conforme à ce qu'on attend !

## Tests sur les erreurs des services

Lister tous les cas de retour d'erreur du service, ainsi que les messages
de l'API

### Exemple

```
-- Tests fonctionnels du Service {'nom du service'} : NOK --
    Service.{'fonction principale'}() : FAILED
        * Exceptions de l'API
            Le parametre '{param}' est obligatoire !
            La reponse est vide !?
            Problème lors de l'analyse de la réponse du service !?
            Une erreur est survenue lors de l'analyse de la réponse json du service !
            (...)
        * Exceptions du service
            Erreur 403
            Erreur 404
            (...)
```

### AVANCEMENTS

--------------------------------------------------------------------------------------------
|  MODE	  | ALTI 	| GEOCODE 	| REVERSE 	| ROUTE 	| ISO 	| AUTOCONF 	| AUTOCOMPLETE 	| SERVICE 	|
|:------:	|:----:	|:-------:	|:-------:	|:-----:	|:---:	|:--------:	|:------------:	|:---------:|
|    XHR	|  (X) 	|   (X)    	|   (X)    	|  (X)   	| (X)  	|     -   	|      (X)     	|     (X)   |
|   JSONP	|   X  	|    X    	|    X    	|   /   	|  /  	|     -   	|       X     	|      X    |

**Note**
> LE XHR EST NON FONCTIONNEL CAR IL FAUDRAIT UN PROXY POUR EXECUTER LES TESTS !

#### Liste des messages
```
'<ExceptionReport><Exception exceptionCode="MissingParameter">Request parameter not found</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">Key parameter not found</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">Service parameter not found</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">Resource parameter not found</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">BAD Request for getCapabilities</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">Bad Request</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingParameter">Key does not exist or has expired</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">Key is in the blacklist</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">Wrong referer</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">Wrong IP Adress</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">Wrong User-Agent</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">No rights for this request</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="MissingRights">No rights for this ressource or ressource does not exist</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="InvalidContentType">Wrong Content-Type</Exception></ExceptionReport>'
'<ExceptionReport><Exception exceptionCode="InvalidParameterValue">replace</Exception></ExceptionReport>'

'<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/ows/1.1 http://schemas.opengis.net/ows/1.1.0/owsExceptionReport.xsd" version="1.0.0" xml:lang="fr"><ows:Exception exceptionCode="MissingParameterValue" locator="identifier"/></ows:ExceptionReport>'
'<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/ows/1.1 http://schemas.opengis.net/ows/1.1.0/owsExceptionReport.xsd" version="1.0.0" xml:lang="fr"><ows:Exception exceptionCode="InvalidParameterValue" locator="identifier"><ows:ExceptionText>Identifier ne peut être vide</ows:ExceptionText></ows:Exception></ows:ExceptionReport>'
'<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/ows/1.1 http://schemas.opengis.net/ows/1.1.0/owsExceptionReport.xsd" version="1.0.0" xml:lang="fr"><ows:Exception exceptionCode="InvalidParameterValue"><ows:ExceptionText>Processus inconnu : processtoreplace</ows:ExceptionText></ows:Exception></ows:ExceptionReport>'
'<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/ows/1.1 http://schemas.opengis.net/ows/1.1.0/owsExceptionReport.xsd" version="1.0.0" xml:lang="fr"><ows:Exception exceptionCode="InvalidParameterValue" locator="identifier"><ows:ExceptionText>Seule l\'opération GetValidityArea est supportée en GET</ows:ExceptionText></ows:Exception></ows:ExceptionReport>'
'<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/ows/1.1 http://schemas.opengis.net/ows/1.1.0/owsExceptionReport.xsd" version="1.0.0" xml:lang="fr"><ows:Exception exceptionCode="OperationNotSupported" locator="identifier"><ows:ExceptionText>processtoreplace n\'est pas un processus reconnu</ows:ExceptionText></ows:Exception></ows:ExceptionReport>'

```

## Comment ajouter des ressources statiques (ex. fichiers xml ou json)

cf. test/spec-functional/test.autoconf.js

### Exemple

Chargement de plusieurs fichiers XML :
```
define(['chai', 'sinon',
    'text!../../test/spec-functional/autoconf-minify.xml',
    'text!../../test/spec-functional/autoconf-beautify.xml'],
function (chai, sinon, autoconfminify, autoconfbeautify) {});
```
