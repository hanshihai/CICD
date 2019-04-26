#Certificate

Anytime a client will connect to the server, server will present its certificate stored in KeyStore and client will verify that certificate by comparing with certificates stored on its trustStore.

Read more: http://www.java67.com/2012/12/difference-between-truststore-vs.html#ixzz51bKCenf9

###Check certificate details
 openssl x509 -in xxx.crt -text -noout   //check cert details

###View public certificate
 openssl s_client -showcerts -connect host:port > host.cert < /dev/null
 cat hosts.cert | openssl x509 -text -noout
 
###import cer into truststore
 keytool -importcert -file certificate.cer -keystore truststore.jks -alias "Alias"

###import pem into truststore
 openssl x509 -outform der -in certificate.pem -out certificate.der
 keytool -import -alias your-alias -keystore cacerts -file certificate.der

###get cert
 openssl s_client -connect host:port </dev/null
 
if you are interested only in the certificate part, cut it out by piping it to:
 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'
and redirect to a file:
 > ${HOST}.cert

Then import it using keytool:
keytool -import -noprompt -trustcacerts -alias ${HOST} -file ${HOST}.cert \
    -keystore ${KEYSTOREFILE} -storepass ${KEYSTOREPASS}
In one go:
HOST=myhost.example.com
PORT=443
KEYSTOREFILE=dest_keystore
KEYSTOREPASS=changeme

### get the SSL certificate
openssl s_client -connect ${HOST}:${PORT} </dev/null \
    | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ${HOST}.cert

### create a keystore and import certificate
keytool -import -noprompt -trustcacerts \
    -alias ${HOST} -file ${HOST}.cert \
    -keystore ${KEYSTOREFILE} -storepass ${KEYSTOREPASS}

### verify
keytool -list -v -keystore ${KEYSTOREFILE} -storepass ${KEYSTOREPASS} -alias ${HOST}

### Add crt into keystore

openssl pkcs12 -export -in xxx.crt -inkey xxx.key -certfile xxx.crt -name hostname -out /tmp/keystore.p12 -password pass:xxx
keytool -importkeystore -srckeystore /tmp/keystore.p12 -srcstoretype pkcs12 -destkeystore .appkeystore -deststoretype JKS -deststorepass xxx -srcstorepass xxx -noprompt

openssl s_client -showcerts -connect host:port > hostname.cert < /dev/null

### Add cert into truststore

keytool -import -noprompt -trustcacerts -alias alianame -file xxx.cert -keystore truststore.jks -storepass xxx