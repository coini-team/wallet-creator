# Formas para crear una wallet con Ethers.js
La siguiente información está detallada en la sección de [Wallet](https://docs.ethers.org/v6/api/wallet/#Wallet "Wallet") de la documentación de [Ethers.js](https://docs.ethers.org/v6/ "Ethers.js"). Se considera el uso de Node.js.

### Crear una wallet con el método createRandom()

Partiendo del siguiente código:
```
    const ethers = require('ethers')
    const wallet = ethers.Wallet.createRandom()
```
podemos crear una wallet y obtener su **address**, **mnemonic**, **private key** y **public key**.

```
console.log('address:', wallet.address);
//address: 0x71CB05EE1b1F506fF321Da3dac38f25c0c9ce6E1

console.log('mnemonic:', wallet.mnemonic.phrase);
//mnemonic: announce room limb pattern dry unit scale effort smooth jazz weasel alcohol

console.log('privateKey:', wallet.privateKey);
//privateKey: 0x1da6847600b0ee25e9ad9a52abbd786dd2502fa4005dd5af9310b7cc7a3b25db

console.log('publicKey:', wallet.publicKey);
//publicKey: 0x02b9e72dfd423bcf95b3801ac93f4392be5ff22143f9980eb78b3a860c4843bfd0
```

Es de notar que la propiedad `.publicKey` retorna la versión [compresa](https://docs.ethers.org/v6/api/wallet/#HDNodeWallet-publicKey "compresa") de la public key. Esto debido a las nuevas prácticas durante el proceso de creación de las keys. Ver [referencia](https://ethereum.stackexchange.com/questions/103482/what-is-the-difference-between-compressed-and-uncompressed-public-key "referencia").

### Crear una o más wallet jerárquicas (HD wallets)


Una billetera que tiene la capacidad de generar una lista larga y casi infinita de addresses a partir de un único método de acceso se llama billetera **HD** (Hierarchical Deterministic wallet). Cuando estas billeteras se crean por primera vez, se genera una frase mnemonica (también conocida como frase semilla) que sirve como raíz de la que surgirán todas las rutas y direcciones.

Hay 2 maneras prácticas de crear una billetera HD, la primera es partiendo de una frase mnemonica ya establecida por nosotros o partiendo de una creada al azar.

1. Caso en que la frase mnemonica ya está establecida
```
const ethers = require('ethers')
const mnemonic = "announce room limb pattern dry unit scale effort smooth jazz weasel alcohol"
const hdNode = ethers.HDNodeWallet.fromPhrase(mnemonic);
const secondAccount = hdNode.derivePath(`m/44'/60'/0'/0/1`);
//0x0AD6daaBF2A0395D773A6a15dbc99C04323Ff182
```

2.  Caso en que la frase mnemonica se genera al azar

```
const ethers = require('ethers')
const wallet = ethers.Wallet.createRandom()
const words = wallet.mnemonic.phrase;

const hdNode = ethers.HDNodeWallet.fromPhrase(mnemonic);
const secondAccount = hdNode.derivePath(`m/44'/60'/0'/0/1`);
//0x71CB05EE1b1F506fF321Da3dac38f25c0c9ce6E1
```
