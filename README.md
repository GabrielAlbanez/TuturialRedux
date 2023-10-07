# TuturialRedux
Como usar redux






Configurando a estrutura do projeto:
Crie uma pasta chamada "store".

Dentro da pasta "store", crie um arquivo chamado "index.ts" e uma pasta chamada "slices" para armazenar os slices dos reducers.

Configurando o arquivo "store/index.ts":

# store/index.ts

import { configureStore } from "@reduxjs/toolkit";
import { useSelector, TypedUseSelectorHook } from "react-redux";
import { Animes } from "./slices/player";

// Configuração da store do Redux
export const store = configureStore({
  reducer: {
    Animes: Animes,
  },
});

// Define o tipo RootState para tipar o estado global
export type RootState = ReturnType<typeof store.getState>;

// Configura o hook useSelector com o tipo RootState
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;


--------------------------------------------------------------------

"store/index.ts":
O arquivo "index.ts" é responsável por configurar a store do Redux, incluindo a definição dos reducers.

A função configureStore é usada para criar a store do Redux com a configuração necessária.

O tipo RootState é definido para tipar o estado global da aplicação, permitindo o uso de auto completions e verificação de tipos quando acessamos o estado.

O hook useAppSelector é configurado para ser usado com o tipo RootState, garantindo que os valores do estado estejam devidamente tipados quando acessados.

Criando um Slice no arquivo "store/slices/Animes.ts":

 # store/slices/Animes.ts

import { createSlice } from "@reduxjs/toolkit";

const playerSlice = createSlice({
  name: "Animes",
  initialState: {
    animes: [
      {
        id: "1",
        nome: "One Piece",
      },
      {
        id: "2",
        nome: "Demon Slayer",
      },
    ],
  },
  reducers: {
    trocarAnime: (state, action) => {
      state.animes[0].nome = action.payload[0];
      state.animes[1].nome = action.payload[1];
    },
  },
});

export const Animes = playerSlice.reducer;
export const { trocarAnime } = playerSlice.actions;



--------------------------------------------------------------------------------------------


"store/slices/Animes.ts":
Neste arquivo, é criado um slice usando createSlice. O slice contém o nome "Animes" e um estado inicial que possui uma lista de animes.

O reducer "trocarAnime" é definido para atualizar o estado com base nos valores fornecidos no payload da action. Ele pega os valores do payload e os atribui aos animes no estado.

Como despachar a action "trocarAnime":

// Exemplo de como despachar a action trocarAnime em algum componente
import { useDispatch } from "react-redux";
import { trocarAnime } from "../store/slices/Animes";

// ...

const dispatch = useDispatch();

// Para despachar a action, use o dispatch
const onPlay = () => {
  dispatch(trocarAnime(["One Punch Man", "My Hero Academy"]));
};

----------------------------------------------------------------------------------------------------


Como consumir os valores do estado global:

// Importando o hook useAppSelector configurado no "store/index.ts"
import { useAppSelector } from "../store";

// Consumindo valores do estado global usando useAppSelector
const modulo = useAppSelector((state) => state.Animes.animes[0].nome);

o state seria o nosso store
dentro do store quer pegar o slice Animes
dentro dele vo acessa o initialState no caso um array Animes
quero pegar desse indice 0 do arry o nome dese objeto


 //pq o selector e bom ele so vai atualizar o componente caso aconteça alteração no dado especific que vc ta pegando no caso ali o "modules"


  //ja na context api o componente ele muda caso o valor do contexto no geral mudar tmb

