# Resgate de helicoptero
Lucas Colonna Romano da Silva - 11202020710

Esse projeto é um jogo de precisão e habilidade! Nele o jogador deve tentar se aproximar o máximo possível do alvo e ganha pontos por cada segundo que permanece próximo a ele!

Para implementar a sensação de movimentação do helicóptero foi criado o seguinte bloco de código: 

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

A carga do modelo é feita através de um arquivo `.obj`. Os shaders são baseados na atividade `viewer6` e como a sensação de movimento é feita pela alteração da `viewMatrix` não houve a necessidade de altera-los. Para o cenário foi implementado um skybox. 

Para a geração do alvo e sua movimentação foi implementado o seguinte `.vert`:

```
#version 300 es

layout(location = 0) in vec3 inPosition;

uniform float angle;
uniform vec3 position;

void main() {
  float sinAngle = sin(angle);
  float cosAngle = cos(angle);

  gl_Position =
    vec4(inPosition.x * cosAngle + inPosition.z * sinAngle + position.x, inPosition.y + position.y,
         inPosition.z * cosAngle - inPosition.x * sinAngle + position.z, 1.0);
}
``` 
Esse vertex shader é mais simples que implementado pelo helicopetero, mas inclui a variável `gl_Position` que é a responsavél pela movimentação do objeto. Aqui o objeto é renderizado em outros pontos da tela e não a `viewMatriz`.

A movimentação do alvo é feita em toda atualização da tela por meio de: 

```
m_position = m_position + glm::vec3(0.0f, 0.0f, 0.0f);

   //Move along the Y-axis with a speed of 0.2 units per second
   if (m_position.y <= 1.0f && reachedTop == 0) {
      m_position = m_position + glm::vec3(0.0f, 0.2f*deltaTime*m_speed, 0.0f);  

      if (m_position.y >= 0.75f) {
        reachedTop = 1;
      }
   }

   if (reachedTop == 1) {
      m_position = m_position + glm::vec3(0.0f, -0.2f*deltaTime*m_speed, 0.0f);  

      if (m_position.y <= -0.75f) {
        reachedTop = 0;
      }
   }

   //Move along the X-axis with a speed of 0.2 units per second
   if (m_position.x <= 1.0f && reachedSide == 0) {
      m_position = m_position + glm::vec3(0.15f*deltaTime*2, 0.0f, 0.0f);  

      if (m_position.x >= 0.75f) {
        reachedSide = 1;
      }
   }

   if (reachedSide == 1) {
      m_position = m_position + glm::vec3(-0.15f*deltaTime*2, -0.0f, -0.0f);  

     if (m_position.x <= -0.75f) {
        reachedSide = 0;
      }
   }
```
A variável `m_position` é incrementada de acordo com o tempo e as variáveis auxiliares `reachedTop` e `reachedSide` invertem o sentido do movimento, fazendo com que o objeto se mantenha dentro das boundaries da tela. 

Por fim a pontuação é contabilizada com 
```
 if (glm::all(glm::epsilonEqual(position, m_position, 0.25f))) {
     points += 1;
  }
```
Essa condicional verifica se os vetores posição de cada objeto estão em uma margem de 0.25f um do outro.
Essa pontuação é então mostrada no `onPaintUI`.

## License

ABCg is licensed under the MIT License. See [LICENSE](https://github.com/hbatagelo/abcg/blob/main/LICENSE) for more information.
