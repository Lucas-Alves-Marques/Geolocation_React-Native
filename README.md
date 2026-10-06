# geolocaliza 📍

Aplicativo de demonstração de geolocalização feito com React Native e Expo.

## Sobre o projeto 🎓

Este projeto foi desenvolvido durante minhas aulas de **Programação Mobile** como forma de estudar como funciona o serviço de geolocalização nos smartphones. A aplicação solicita acesso à localização em primeiro plano, obtém a posição atual do aparelho e acompanha as mudanças de posição, mostrando-as em um mapa.

O foco é didático: observar o uso das permissões do sistema, a leitura das coordenadas e a atualização de uma interface móvel com dados de localização.

## Funcionalidades 🗺️

- Solicita permissão para acessar a localização enquanto o aplicativo está em uso.
- Obtém uma posição inicial com `expo-location`.
- Exibe um mapa centrado nas coordenadas obtidas.
- Mostra a posição atual com um marcador.
- Acompanha atualizações de localização e atualiza a posição do marcador.
- Anima a câmera do mapa para acompanhar o aparelho, com inclinação (`pitch`) de 70 graus.
- Registra a posição inicial e as atualizações no console de desenvolvimento.

O acompanhamento é configurado para alta precisão, com solicitação de atualização a cada segundo ou após um metro de deslocamento. O sistema operacional e o dispositivo podem influenciar a frequência efetivamente recebida.

## Tecnologias e dependências 🧰

- **Expo SDK 52** e **React Native 0.76**
- **React 18** e **TypeScript**
- [`expo-location`](https://docs.expo.dev/versions/v52.0.0/sdk/location/) para permissão e leitura da localização
- [`react-native-maps`](https://github.com/react-native-maps/react-native-maps) para mapa e marcador
- `StyleSheet` do React Native para os estilos

## Requisitos 📱

- Node.js e npm instalados.
- Um smartphone Android com serviços de localização/GPS ou um emulador Android configurado.
- Para executar a versão nativa pelo Android, Android Studio e Android SDK configurados.
- Para executar em iOS, um Mac com Xcode configurado.

Para testar em um aparelho físico, o computador e o celular devem conseguir se comunicar pela rede. O aparelho também precisa permitir o acesso à localização para o aplicativo.

## Instalação e execução ▶️

Os arquivos do aplicativo ficam na pasta `geolocaliza`. No terminal, entre nela e instale as dependências:

```bash
cd geolocaliza
npm ci
```

Inicie o servidor de desenvolvimento do Expo:

```bash
npm start
```

Com o servidor iniciado, use o QR code exibido no terminal para abrir o projeto no Expo Go compatível com o SDK do projeto, ou escolha um emulador/dispositivo conectado pelas opções apresentadas pelo Expo.

### Android

Para executar em um emulador ou aparelho Android com o ambiente nativo configurado:

```bash
npm run android
```

Esse comando executa `expo run:android` e compila/instala o aplicativo nativo. Na primeira execução, confirme a solicitação de localização no aparelho.

### iOS

Em um Mac com Xcode instalado e dispositivo ou simulador disponível:

```bash
npm run ios
```

Esse comando executa `expo run:ios`. A execução nativa para iOS não pode ser compilada diretamente no Windows.

### Web

O projeto também define o comando:

```bash
npm run web
```

Ele inicia a versão web pelo Expo. O aplicativo foi desenvolvido com foco em dispositivos móveis; como `react-native-maps` é voltado às plataformas nativas, o mapa e as funcionalidades relacionadas podem não estar disponíveis ou se comportar de forma diferente no navegador.

## Como funciona 🧭

1. Ao montar o componente principal, `requestLocationPermissions()` solicita permissão em primeiro plano usando `requestForegroundPermissionsAsync()`.
2. Se a permissão for concedida, `getCurrentPositionAsync()` obtém a posição inicial e a salva no estado `location`.
3. O mapa só é renderizado quando existe uma localização disponível. Sua região inicial usa latitude e longitude atuais, com deltas de `0.005`.
4. `watchPositionAsync()` acompanha mudanças de posição usando `LocationAccuracy.Highest`, `timeInterval: 1000` e `distanceInterval: 1`.
5. A cada atualização recebida, o aplicativo atualiza `location` e anima a câmera para as novas coordenadas.
6. O marcador é renderizado nas coordenadas da localização mais recente.

Se a permissão for negada, a posição inicial não é definida e, portanto, o mapa não aparece. Para testar a funcionalidade, conceda a permissão e mantenha os serviços de localização do aparelho habilitados.

## Estrutura do projeto 🗂️

```text
Geolocation_React-Native/
├── README.md
└── geolocaliza/
    ├── App.tsx
    ├── app.json
    ├── index.ts
    ├── styles.ts
    ├── package.json
    ├── package-lock.json
    ├── assets/
    └── android/
```

- **`App.tsx`**: componente principal; gerencia permissão, localização, acompanhamento contínuo, mapa e marcador.
- **`styles.ts`**: estilos da tela e do mapa. O mapa ocupa toda a área disponível.
- **`index.ts`**: registra `App` como componente raiz do aplicativo Expo.
- **`app.json`**: configura nome, orientação, ícones, splash screen e identificador Android.
- **`package.json`**: dependências e comandos de execução.
- **`assets/`**: ícones, favicon e imagem da tela de abertura.
- **`android/`**: projeto nativo Android. O manifesto declara as permissões de localização aproximada e precisa; a autorização do usuário é solicitada durante a execução.

## Scripts disponíveis ⚙️

| Comando | Ação |
| --- | --- |
| `npm start` | Inicia o servidor de desenvolvimento do Expo. |
| `npm run android` | Compila e executa o aplicativo nativo Android. |
| `npm run ios` | Compila e executa o aplicativo nativo iOS em macOS com Xcode. |
| `npm run web` | Inicia o projeto na plataforma web do Expo. |

## Observações 🔎

- A aplicação solicita somente localização em primeiro plano; não implementa rastreamento em segundo plano.
- O mapa depende da disponibilidade de uma localização inicial para ser exibido.
- A posição e a frequência das atualizações dependem do dispositivo, dos serviços de localização e das permissões concedidas.
