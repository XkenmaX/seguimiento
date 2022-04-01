
X es una plataforma basada en el progreso de integración de [ OTP ](https://www.transunion.co/producto/otp "OTP") que posee [ Nequi ](https://www.nequi.com. co/?gclid=CjwKCAjwopWSBhB6EiwAjxmqDbxnrC7casaQcyJO4lLV0E2HkJRpbojMxvvTvh-Cmusz22UprB19wRoC9p8QAvD_BwE "Nequi").


Gracias a esta integración nos permitirá recibir pagos de los clientes a nuestra plataforma me diente una OTP dentro de nuestra aplicación web, esta OTP debe ser entregada al comercio para que este realice un pago por determinado valor mediante una API.

El estandar esta compuesto por tres servicios REST usando mensajeria JSON y usando un mecanismo de autenticación basado en [ OAuth 2.0 ](https://oauth.net/2/ "OAuth 2.0")
El estandar esta compuesto por tres servicios REST usando mensajeria PHP y usando un mecanismo de autenticación basado en [ OAuth 2.0 ](https://oauth.net/2/ "OAuth 2.0")



@@ -14,7 +13,4 @@ Los servicios expuestos en esta API están autenticados mediante el mecanismo de
Para poder consumirlos necesitará obtener un token de acceso y enviarlo en la cabecera Authorization precedido de la palabra Bearer (Portadora).

    {
        "Señal_acceso": "eyJraWQiOiJuZVhiaFBIVkREV3IxXC9sZTl2YVdVQ0laNHlrSHZsUkF0bjFGajBRSVU3WT0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI1azY1N2gxOTBicG9xajE3cGZ1ZXN0a3FrcSIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoib3RoZXJcL290aGVyIiwiYXV0aF90aW1lIjoxNTY1MDYyMTM1LCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtZWFzdC0xLmFtYXpvbmF3cy5jb21cL3VzLWVhc3QtMV9tNm9tT0JxQ1kiLCJleHAiOjE1NjUwNjU3MzUsImlhdCI6MTU2NTA2MjEzNSwidmVyc2lvbiI6MiwianRpIjoiYmQ2NGZlY2EtMTIxOS00NDhmLTkxN2UtMmUyYmU0NDgwNWE3IiwiY2xpZW50X2lkIjoiNWs2NTdoMTkwYnBvcWoxN3BmdWVzdGtxa3EifQ.IryJj8pCqcW9vJ-V-FVjA8f9AXFuKeVsOMA0GD61r4hkmgC-DRkNhcpFTqfqwFcxUk_7NVhEvIzCDelaI_hdODr6s1MuhVLWSa1PoompU1ZEDrQn2L5aR-rfdNaRH7mWI4DR_xRjIghoWOMgKPfkz1KgZx1FmeXZaD3cCFcxLb7zIR_aWC9az5s3tpoFPMjBXY_NtNOL6RmRZPJqlQJ0CwB9qPsvNDFStoSIxTqLOuvwF7vIJBt-3Nf5IjSRDP1KVUEYSe2RlmTxEcsrCU1QOQKUMhuejhE9_EHF7BPqBGeJWu3OXtNKwad_cimoWHyfAmGFHhggyoe1E3DXDSuWYg",
        "expires_in": 3600,
        "token_type": "Portador"
    }
        "Señal_acceso": "eyJraWQiOiJuZVhiaFBIVkREV3IxXC9sZTl2YVdVQ0laNHlrSHZsUkF0bjFGajBRSVU3WT0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI1azY1N2gxOTBicG9xajE3cGZ1ZXN0a3FrcSIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoib3RoZXJcL290aGVyIiwiYXV0aF90aW1lIjoxNTY1MDYyMTM1LCJpc3MiOiJodHRwczpcL1wvY29nbml0by1p
