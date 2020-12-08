<h1 align="center">
    <img alt="Image Trybe" src="https://i.ibb.co/d4W2x4g/trybe.png" width="400px" />
</h1>

<h3 align="center">
  Exercício 16-2: Usando o Redux no React--part-3(final) - Concluído o/ o/ o/ :star:
</h3>

<blockquote align="center">“Quanto mais você estuda, mais aprende e se aproxima de realizar seu sonhos!”</blockquote>

<h1></h1>

<p align="center">

  <a href="https://www.linkedin.com/in/eduardosouzaprogrammer/">
    <img alt="Made by Eduardo Souza" src="https://img.shields.io/badge/made%20by-Edu%20Souza-%23F8952D">
  </a>&nbsp;

 <a href="https://edusouza-programmer.github.io/">
<img alt="Github page Edu Souza " src="https://img.shields.io/badge/Github%20page-Edu_Souza-orange">
</a>&nbsp;

  <a href="LICENSE" >
    <img alt="License" src="https://img.shields.io/badge/license-MIT-%23F8952D">
  </a>

</p>

<p align="center">
  <a href="#rocket-Sobre-o-Exercício">Sobre o Exercício</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#postbox-Entrega">Entrega</a>&nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#unlock-Licença">Licença</a>
</p>

# :rocket: Sobre o Exercício

Redux é uma ferramenta para gerenciar o estado de uma aplicação JavaScript . Antes de entender o porquê de utilizar React com Redux , deve-se entender o porquê de utilizarmos uma biblioteca externa para controlar e gerenciar o estado de uma aplicação. A maioria das bibliotecas, como React , Angular etc, possuem uma forma interna de gerenciar o estado da aplicação sem o auxílio ou necessidade de uma ferramenta externa. Isto funciona bem para aplicações que possuem poucos componentes mas, à medida que a aplicação cresce, o gerenciamento de estados compartilhados entre componentes torna-se uma tarefa complicada e desgastante.

# :postbox: Entrega

Você irá desenvolver 3 exercícios para solidificar seus conhecimentos de Redux com React.

- No primeiro exercício, desenvolveremos um semáforo simples, [veja parte 1.](https://github.com/EduSouza-programmer/Trybe_Exercicio_16-2_Edu_Souza--part-1)
- No segundo trabalharemos com mais estados aplicando movimento em 3 carros, [veja parte 2.](https://github.com/EduSouza-programmer/Trybe_Exercicio_16-2_Edu_Souza--part-2)
- E para finalizar iremos combinar ambos os exercícios em um só, utilizando o combineReducers.

### :clipboard: Sumário

- <p><a href="#3"> :pushpin: 3.</a> Crie um array com strings no formato NOME_DO_LIVRO - GÊNERO_DO_LIVRO - NOME_DA_PESSOA_AUTORA;</p>

## :books: Exercícios

### 3°

- Nesse exercício utilizaremos os códigos dos exercícios 1 e 2. Aqui vamos juntar (combinar) os reducers dos dois primeiros exercícios, para que eles possam ser carregados juntos na mesma página da aplicação.

- O funcionamento dos dois componentes **DEVE** se manter o mesmo.

- Nesse exercício toda a `store` já está pronta, você precisará criar os arquivos para cada reducer e mover o código do reducer de cada exercício para um arquivo diferente, utilizar o arquivo `redux/index.js` para montar sua `store` e combinar os dois reducers. Além de alterar os componentes `Cars.jsx` e `TrafficSignal.jsx` para receberem seus respectivos reducers.

- Utilize **Redux** para armazenar todo o estado da aplicação.

#### Resposta:

<details>
 <summary> :pencil2: Código Javascript</summary>

```js
// src/index.js
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import App from "./App";
import store from "./redux";

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);

/* -------------------- */

// src/App.jsx
import React from "react";
import TrafficSignal from "./TrafficSignal";
import Cars from "./Cars";
import "./App.css";

export default function App() {
  return (
    <div className="ctn">
      <TrafficSignal />
      <Cars />
    </div>
  );
}

/* -------------------- */

// src/redux/index.js
import { createStore, combineReducers } from "redux";
import trafficReducer from "./trafficReducer";
import carReducer from "./carReducer";

const rootReducer = combineReducers({ trafficReducer, carReducer });

const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

export default store;

/* -------------------- */

// src/redux/actionCreators.js
export const CHANGE_SIGNAL = "CHANGE_SIGNAL";
export const MOVE_CAR = "MOVE_CAR";

export const changeSignal = (payload) => ({
  type: CHANGE_SIGNAL,
  payload,
});

export const moveCar = (car, side) => ({
  type: MOVE_CAR,
  car,
  side,
});

/* -------------------- */

// src/redux/carReducer.js
import { MOVE_CAR } from "./actionCreators";

const initialState = {
  cars: {
    red: false,
    blue: false,
    yellow: false,
  },
};

function carReducer(state = initialState, action) {
  switch (action.type) {
    case MOVE_CAR:
      return {
        ...state,
        cars: { ...state.cars, [action.car]: action.side },
      };

    default:
      return state;
  }
}

export default carReducer;

/* -------------------- */

// src/redux/trafficReducer.js
import { CHANGE_SIGNAL } from "./actionCreators";

const initialState = {
  signal: {
    color: "red",
  },
};

function trafficReducer(state = initialState, action) {
  switch (action.type) {
    case CHANGE_SIGNAL:
      return { ...state, signal: { ...state.signal, color: action.payload } };

    default:
      return state;
  }
}

export default trafficReducer;

/* -------------------- */

// src/Cars.jsx
import React from "react";
import { bool, func } from "prop-types";
import { connect } from "react-redux";
import { moveCar } from "./redux/actionCreators";
import carBlue from "./images/carBlue.jpeg";
import carRed from "./images/carRed.jpeg";
import carYellow from "./images/carYellow.jpeg";

function Cars({ redCar, blueCar, yellowCar, moveCar }) {
  return (
    <div>
      <div>
        <img
          className={redCar ? "car-right" : "car-left"}
          src={carRed}
          alt="red car"
        />
        <button onClick={() => moveCar("red", !redCar)} type="button">
          move
        </button>
      </div>
      <div>
        <img
          className={blueCar ? "car-right" : "car-left"}
          src={carBlue}
          alt="blue car"
        />
        <button onClick={() => moveCar("blue", !blueCar)} type="button">
          move
        </button>
      </div>
      <div>
        <img
          className={yellowCar ? "car-right" : "car-left"}
          src={carYellow}
          alt="yellow car"
        />
        <button onClick={() => moveCar("yellow", !yellowCar)} type="button">
          move
        </button>
      </div>
    </div>
  );
}

const mapStateToProps = (state) => ({
  redCar: state.carReducer.cars.red,
  blueCar: state.carReducer.cars.blue,
  yellowCar: state.carReducer.cars.yellow,
});

/* const mapDispatchToProps = (dispatch) => ({
  moveCar: (car, side) => dispatch(moveCar(car, side)),
}); */

// Podemos também fazer assim :
// Isso porque temos o mesmo nome da action e a função de callback no evento.
const mapDispatchToProps = { moveCar };

Cars.propTypes = {
  redCar: bool.isRequired,
  blueCar: bool.isRequired,
  yellowCar: bool.isRequired,
  moveCar: func.isRequired,
};

export default connect(mapStateToProps, mapDispatchToProps)(Cars);

/* -------------------- */

// src/TrafficSignal.jsx
import React from "react";
import { string, func } from "prop-types";
import { connect } from "react-redux";
import { changeSignal } from "./redux/actionCreators";
import redSignal from "./images/redSignal.jpeg";
import greenSignal from "./images/greenSignal.jpeg";
import yellowSignal from "./images/yellowSignal.jpeg";

const renderSignal = (signalColor) => {
  if (signalColor === "red") {
    return redSignal;
  }
  if (signalColor === "green") {
    return greenSignal;
  }
  if (signalColor === "yellow") {
    return yellowSignal;
  }
  return null;
};

function TrafficSignal({ signalColor, changeSignal }) {
  return (
    <div>
      <div className="button-container">
        <button onClick={() => changeSignal("red")} type="button">
          Red
        </button>
        <button onClick={() => changeSignal("yellow")} type="button">
          Yellow
        </button>
        <button onClick={() => changeSignal("green")} type="button">
          Green
        </button>
      </div>
      <img className="signal" src={renderSignal(signalColor)} alt="" />
    </div>
  );
}

const mapStateToProps = (state) => ({
  signalColor: state.trafficReducer.signal.color,
});

const mapDispatchToProps = {
  changeSignal,
};

TrafficSignal.propTypes = {
  signalColor: string.isRequired,
  changeSignal: func.isRequired,
};

export default connect(mapStateToProps, mapDispatchToProps)(TrafficSignal);
```

</details>

<p align="right">
   <a href="https://edusouza-programmer.github.io/Trybe_Exercicio_3-3_Edu_Souza//parte-1/challenge_1-o_modelo_boxer.html">
    <img alt="Go index.html" src="https://img.shields.io/badge/Go-app_react-orange">
    </a>&nbsp;
    <a href="#clipboard-Sumário">
    <img alt="Back Sumário" src="https://img.shields.io/badge/Back-Sum%C3%A1rio-orange">
  </a>
</p>

#

## :unlock: Licença

Este projeto está licenciado sob a Licença MIT - consulte [LICENSE](https://opensource.org/licenses/MIT) para maiores detalhes.
