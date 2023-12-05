# Resgate de helicoptero
Lucas Colonna Romano da Silva - 11202020710


Nosso projeto envolve simular o voo de um helicóptero de resgate, em que o usuário pode movimentar o modelo dentro do cenário. 

Para implementar a sensação de movimentação foi criado o seguinte bloco de código: 

```if (event.type == SDL_KEYDOWN) {
      if (event.key.keysym.sym == SDLK_DOWN) {
          position.y -= 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_UP) {
          position.y += 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_LEFT) {
          position.x -= 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_RIGHT) {
          position.x += 0.1f;
      }
  }
  else if (event.type == SDL_KEYUP) {
      if (event.key.keysym.sym == SDLK_DOWN) {
          position.y += 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_UP) {
          position.y -= 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_LEFT) {
          position.x += 0.1f;
      }
      else if (event.key.keysym.sym == SDLK_RIGHT) {
          position.x -= 0.1f;
      }
  }
```

Esse bloco de código permite a captura de eventos do teclado e atualiza o vetor `position`. Esse vetor é então usado na `viewMatriz` para criar a sensação de movimento, como segue: 
```
 m_viewMatrix =
      glm::lookAt(glm::vec3(0.0f, 0.0f, 1.0f + m_zoom),	glm::lookAt(glm::vec3(0.0f, 0.0f, 2.0f + m_zoom),
                  glm::vec3(0.0f, 0.0f, 0.0f), glm::vec3(0.0f, 1.0f, 0.0f));
      glm::vec3(0.0f, 0.0f, 0.0f) + position, glm::vec3(0.0f, 1.0f, 0.0f));
}
```

A carga do modelo é feita através de um arquivo `.obj` e segue também a implementação de um skybox. 

## License

ABCg is licensed under the MIT License. See [LICENSE](https://github.com/hbatagelo/abcg/blob/main/LICENSE) for more information.
