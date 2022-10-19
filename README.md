# Teste de hipótese (z-score) para sísmica 4D

Dados: 

* Sísmica Base (*B*), contendo sinal + ruído aleatório
* Sísmica Monitora (*M*), contendo sinal + ruído aleatório
* NRMS medido em região sem efeito 4D
* Impedânica P (*IP*) das camadas 1 (topo/selo) e 2 (base/reservatório), nos cenários base e monitor
* Amplitude de reflexão na interface 1/2 nos cenários base e monitor, a partir da qual é possível calcular a amplitude 4D: $A^{\textrm{(4D)}}$ = $A^{(M)}$ - $A^{(B)}$


Pergunta: Como saber se a amplitude 4D medida é, de fato, causada por mudança de impedância no reservatório?

Estabelecemos então as seguintes premissas:  

**Hipótese Nula ($H_0$)**: A amplitude 4D medida é simplesmente resultado do ruído aleatório, que é diferente nos cenários base e monitor   
**Hipótese Alternativa ($H_a$)**: O sinal 4D é causado por mudança de impedância no reservatório

Iremos utilizar uma das formulações mais simples em testes de hipótese, o teste Z. Para que o teste de inferência seja válido, as medidas devem satisfazer três critérios: **Aleatoriedade**, **Independência** e **Normalidade**.  

### **Aleatoriedade**
Amostras aleatórias devem prover estatísticas não-enviesadas sobre a população amostrada. 

### **Independência**
Cada medida é independente da outra. 

### **Normalidade**
A terceira premissa é de que a amostra selecionada tem distribuição normal. Vamos assumir que a interface que estamos investigando apresenta um contraste de impedância que leva a uma refletividade $R^{(B)}$, no cenário base. Suponde que a não-repetibilidade da sísmica está totalmente contemplada no ruído aleatório, podemos dizer que a amplitude, a menos de um fator de escala, será dada por 

$$
A = R + \mathcal{N}(0,\sigma),
$$

onde $\mathcal{N}(0,\sigma)$ representa uma distribuição normal com média zero e desvio-padrão $\sigma$. Dessa forma, as amplitudes base e monitora serão ambas distribuições normais com desvio-padrão $\sigma$, e médias $R^{(B)}$ e $R^{(M)}$, respectimente. A amplitude 4D será dada por

$$
A^{(\textrm{4D})} = A^{(M)} - A^{(B)} = \mathcal{N}(R^{(M)},\sigma) - \mathcal{N}(R^{(B)},\sigma) = \mathcal{N}(\Delta R,\sigma\sqrt{2}),
$$

em que a propriedade da soma de duas distirbuições normais foi obtida a partir da 
[propriedade da soma de duas distirbuições normais](https://en.wikipedia.org/wiki/Sum_of_normally_distributed_random_variables).

<br />  

***

<br />  
## Amplitude 4D e significância estatística

Dada uma certa medida de amplitude 4D, o primeira passo do teste é calcular o Z-score, ou a estatística Z da amostra. O cálculo do Z-score nada mais é do que a "padronização" da estatística - a aplicação uma transformação linear que transforma a amostra em sua equivalente de média 0 e desvio padrão 1. Para isso, usamos a expressão

$$
Z = \dfrac{\hat{A} - \mu_A}{\sigma_A},
$$

em que $Z$ é o score normalizado, $\hat{A}$ é amplitude 4D medida, ${\mu}_A$ é a amplitude média esperada na ausência de sinal 4D (${\mu}_A=0$) e $\sigma_A$ é o desvio padrão dessa amplitude, que pode ser interpretado como o desvio-padrão do ruído aleatório no dado 4D - ou, usando a expressão obtida anteriormente, $\sigma_A = \sigma\sqrt{2}$, em que $\sigma$ é o desvio-padrão do ruído aleatório no dado 3D.

Na prática, é difícil acessar o valor de $\sigma$ - a métrica usada para "ruído" acaba sendo o NRMS. Além disso, em estudos de viabilidade, trabalha-se com "variação de impedância" entre base e monitor, e não variações de amplitude.

A próxima seção visa a estabelecer as relações entre NRMS e ruído aleatório, e entre amplitude 4D e variação de impedância acústica.


<br />  
## NRMS e Níveis de Ruído

Para um dado valor de NRMS, qual o valor de $\sigma$? 

Usando a definição de NRMS,

$$
\textrm{NRMS} = 200 \times \dfrac{\textrm{RMS }(A^{\textrm{(4D)}})}{\textrm{RMS }(A^{(B)}) + \textrm{RMS }(A^{(M)})}
$$

Numa região sem efeitos de produção, não há mudança de refletividade, logo $\Delta R = 0$. Isso implica em $A^{\textrm{(4D)}} = \mathcal{N}(0,\sigma\sqrt{2})$. Assim,

$$
\textrm{NRMS} = 200 \times \dfrac{\textrm{RMS }(\mathcal{N}(0,\sigma\sqrt{2}))}{\textrm{RMS }[\mathcal{N}(R^{(B)},\sigma)] + \textrm{RMS }[\mathcal{N}(R^{(M)},\sigma)]}
$$

Usando [a propriedade do cálculo do RMS para dstribuições normais](https://en.wikipedia.org/wiki/Root_mean_square#Relationship_to_other_statistics),

$$
\textrm{RMS }(\mathcal{N}(0,\sigma\sqrt{2})) = \sigma\sqrt{2}
$$ 

e

$$ 
\textrm{RMS }(\mathcal{N}(R,\sigma)) = \sqrt{R^2 + \sigma^2}
$$

Dessa forma, o NRMS pode ser expresso como

$$
\textrm{NRMS} = 200 \times \dfrac{\sigma\sqrt{2}}{\sqrt{{R^2}^{(B)} + \sigma^2}\sqrt{{R^2}^{(M)} + \sigma^2}}
$$
 
Para valores baixos de NRMS (intuitivamente: ruído muito menor do que a intensidade da reflexão 3D), assumiremos $|R|\gg\sigma$, e podemos aproximar $\sqrt{R^2 + \sigma^2} \approx |R|$. Assim,

$$
\textrm{NRMS} \approx 200 \times \dfrac{\sigma\sqrt{2}}{2|R|} = 100 \times \dfrac{\sigma\sqrt{2}}{|R|}, \qquad \textrm{ou} \qquad \sigma = \dfrac{\textrm{NRMS}}{100\sqrt{2}}|R|
$$

Com base no resultado acima, podemos modelar o nível de ruído para se obter um determinado NRMS. Exemplo: em uma interface cuja amplitude (ou refletividade, a menos de um fator de escala) seja 0.1 no dado 3D (sem ruído), para obter um nivel de NRMS de 3, o desvio-padrão $\sigma$ do ruído Gaussiano a ser adicionado é

$$
\sigma = \dfrac{3}{100\sqrt{2}}\times 100 \approx 2.12
$$
 
Uma vez que a conexão entre NRMS (variável medida no dado de campo) e nível de ruído está estabelecida, o próximo passo é definir uma conexão entre amplitude (ou refletividade) e variação de impedância acústica ($\Delta\textrm{IP}$). Para isso, vamos usar a definição de refletividade *zero-offet*:

$$
R = \dfrac{\textrm{IP}_2 - \textrm{IP}_1}{\textrm{IP}_2 + \textrm{IP}_1}
$$
 
A variação de refletividade $\Delta R$ será

$$
\Delta R = R^{(M)} - R^{(B)} = \dfrac{\textrm{IP}^{(M)}_2 - \textrm{IP}^{(M)}_1}{\textrm{IP}^{(M)}_2 + \textrm{IP}^{(M)}_1} - \dfrac{\textrm{IP}^{(B)}_2 - \textrm{IP}^{(B)}_1}{\textrm{IP}^{(B)}_2 + \textrm{IP}^{(B)}_1}
$$ 
 
Assumindo $ \textrm{IP}^{(B)}_2 + \textrm{IP}^{(B)}_1 \approx \textrm{IP}^{(M)}_2 + \textrm{IP}^{(M)}_1 \gg \textrm{IP}^{(B,M)}_2 - \textrm{IP}^{(B,M)}_1 $, ficamos com

$$
\Delta R \approx \dfrac{1}{\textrm{IP}^{(B)}_2 + \textrm{IP}^{(B)}_1}\left[ \textrm{IP}^{(M)}_2 - \textrm{IP}^{(M)}_1 - \textrm{IP}^{(B)}_2 + \textrm{IP}^{(B)}_1\right] = \dfrac{(\textrm{IP}^{(M)}_2 - \textrm{IP}^{(B)}_2) - (\textrm{IP}^{(M)}_1 - \textrm{IP}^{(B)}_1)}{\textrm{IP}^{(B)}_2 + \textrm{IP}^{(B)}_1} = R\dfrac{\Delta\textrm{IP}^{(Produção)}_2 - \Delta\textrm{IP}^{(Produção)}_1}{\Delta\textrm{IP}^{(Original)}}
$$

Agora, conseguimos estabelecer uma relação entre mudaça de amplitude (u refletividade) e mudança esperada de IP na zona de produção. Reparar que essa diferença é depende do contraste inicial de impedância, levantando um ponto que também é intuitivo: uma mudança de 3% de impedância acústica pode levar a resultados diferentes, a depender do contraste inicial entre as camadas.

Por fim, vem a pergunta: ao modelarmos uma certa variação de impedância causada por 4D, e assumirmos um determinado nível de NRMS, qual a chance de esse sinal ser detectado em campo? Para isso, calculamos o Z-score do sinal modelado:

$$
Z = \dfrac{\hat{A} - \mu_A}{\sigma_A} = \dfrac{\Delta R}{\sigma\sqrt{2}} = R\dfrac{\Delta\textrm{IP}^{(Produção)}_2 - \Delta\textrm{IP}^{(Produção)}_1}{\Delta\textrm{IP}^{(Original)}} \times \dfrac{100}{\textrm{NRMS}|R|} = \dfrac{\Delta\textrm{IP}^{(Produção)}_2 - \Delta\textrm{IP}^{(Produção)}_1}{\Delta\textrm{IP}^{(Original)}} \times \dfrac{100}{\textrm{NRMS}}
$$
 
A ultima expressão (na qual assumimos $R$ positivo, mas sem perda de generalidade) pode ser interpretada como uma probabilidade de aquela anomalia ser real, e não apenas ruído estatístico: um Z-score de 3, por exemplo, indicaria 97.5 % de chance de a anomalia ser de fato um sinal 4D, e não apenas flutuação estatística.

<br />  <br />  

### Contato: Filipe Borges (filipeborges@petrobras.com.br)
