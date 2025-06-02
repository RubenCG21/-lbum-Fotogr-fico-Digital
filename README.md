O sistema “Álbum Fotográfico Digital” foi desenvolvido com base numa arquitetura distribuída composta por 
dois clientes distintos, designados por Cliente A e Cliente B, que comunicam entre si através do protocolo 
MQTT, utilizando uma ligação segura com TLS/SSL e autenticação por nome de utilizador e palavra-passe. 
O Cliente A é responsável por capturar imagens em tempo real através de uma webcam, utilizando o módulo 
(node-red-node-ui-webcam) no ambiente Node-RED. Após a captura, o utilizador pode inserir uma descrição 
textual para a imagem ou optar por descartá-la. As imagens aprovadas são publicadas num tópico MQTT 
específico, com o payload a incluir o id da imagem, data, a imagem (em base64) e a descrição. 
O Cliente B subscreve esse tópico e recebe automaticamente as imagens publicadas, armazenando-as 
localmente num array. Na interface gráfica deste cliente, a galeria é exibida em ordem cronológica, 
permitindo ao utilizador visualizar tanto a imagem como a respetiva descrição. 
A arquitetura também contempla mecanismos de gestão da galeria. O Cliente B monitoriza o número total 
de imagens armazenadas e, ao detetar que restam apenas três posições disponíveis (num limite máximo de 
10 imagens), envia um aviso preventivo ao Cliente A. Este aviso é transmitido por MQTT num tópico de 
estado. Caso a galeria atinja o limite máximo de imagens, um alerta é enviado automaticamente ao Cliente 
A através de uma notificação via Telegram e um aviso na interface, solicitando a remoção de uma imagem. 
O Cliente A, ao receber o alerta, pode selecionar o ID da imagem a remover ou renomear uma descrição 
existente e também poderá fazer uma limpeza total à galeria, os comandos de remoção, renomeação e 
limpeza total são enviados ao Cliente B, que atualiza localmente o conteúdo da galeria.
