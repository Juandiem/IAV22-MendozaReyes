# IAV22-Juan Diego Mendoza Reyes
This repository is used for the final project of Artificial Intelligence signature.

Propuesta: Inteligencia Artificial que sea capaz de jugar en equipo a tactical shooters, para este proyecto nos centrariamos en reproducir los roles del
tactical shooter multijugador Valorant, desarrollado por Riot Games, estos roles son :

-El controlador: encargado de colocar humos en el mapa para cancelar la visión del enemigo.

-El centinela: encargado de preveer flanqueos del enemigo y de contrarestarlos.

-El duelista: encargado de comprobar ángulos para su equipo y de entrar a las zonas de plante de la bomba.

-El iniciador: encargado de ayudar al duelista en su objetivo.

Para facilitar el trabajo, utilizaremos las habilidades (simplificadas, quitando físicas como la del lanzamiento de una flecha) de los siguientes personajes de
Valorant. 
Controlador : Brimstone.
Centinela : Cypher
Duelista : Jett.
Iniciador: Sova

El objetivo final de la práctica es que los personajes se comuniquen entre si, utilicen sus habilidades de forma coordinada y, si tuviesemos tiempo, implementar
un machine learning en el que los personajes aprendan, donde se colcoan sus enemigos, como juegan sus compañeros, y que tácticas les son más y menos efectivas


## Resumen
La práctica consiste en implementar una inteligencia artificial que sea capaz de trabajar en equipo junto con otras 3.

En este repositorio implementaremos la inteligencia artificial que hará las funciones de duelista, usando al personaje Jett, del videojuego Valorant. 

Valorant consiste de dos equipos, el atacante y el defensor, el equipo atacante debe plantar una bomba
al lado de las cajas de radianita (un material valioso que permite generar enegergia rápidamente) para destruirla, mientras que el equipo atacante debe impedirlo. Nosotros nos centraremos en implementar
la coordinación del ataque.

<br>
El entorno de la práctica será el sitio de B del mapa Bind de valorant, modelado en Blender y texturizado.

## Rol en la partida
El duelista es aquel que debe comprobar ángulos y esquinas donde pueda esconderse el enemigo para forzar duelos favorables y así lograr la ventaja numérica. Para ello, Jett contará con la habilidad de desplazarse unos metros en horizontal hacia la dirección en la que está mirando de forma instantánea.
Esta habilidad se podrá volver a utilizar si Jett elimina a 2 enemigos.

## Objetivo de la IA
El objetivo de esta IA es que aprenda en que lugares es mas probable que se encuentre un enemigo y en que situaciones es más eficiente el uso del dash.

## Punto de partida
Utilizaremos una plantilla de shooter en primera persona para Unity para ahorrarnos el trabajo de implementar las mecánicas básicas de un shooter.

## Resultado de la paortación individual
Mi aportación a la práctica ha sido la creación del sistema de disparo y el comportamiento total de Jett. Para el comportamiento de Jett, he implementado un sistema de rutas que he predefinifo manualmente en su comportamiento, que sigue una de estas rutas, mientras se para a defender o vigilar la siguiente zona en caso de enemigos.

Jett es un personaje de Valorant de tipo duelista, por tanto su comportamiento en el proyecto será agresivo buscando eliminar el mayor número de enemigos posibles en el menor tiempo posible, al igual que en el juego original. Esta, posee una mayor capacidad de reacción a enemigo que sus compañeros debido a su rol, por lo tanto, tiene un sistema que dedica un menor tiempo a la vigilancia de zonas y más al avance por el mapa y un estado de alerta si se encuentra enemigos, favorecido por el uso de un sistema de percepción visual usando su cámara individual.

Pese a ser duelista, tiene también comunicación con sus compañeros, espoecialmente con Sova, pidiendole una espera cuando ambos se encuentran juntos, para limpiar un zona concreta a la que sova identificará enemigos con su flecha, de forma eficiente. También avisa cuando no hay enemigos en la zona para plantar la spike.

Y por último, en cuanto al comportamiento, Jett dispone de una habilidad Dash, que le permite moverse ágilmente por el escenario pero con usos limitados. Las situaciones más probables a usar el dash son, cuando se encuentra en peligro ante la presencia de enemigos, cuando esta ha terminado su ruta y pasa a otra avanzará rápidamente a esa para evitar demora y cuando Sova detecte un enemigo en una zona que limpian juntos, Jett usará su avance rápido para sorprender a los enemigos y acabra con los enemigos detectados por la flecha.

## Conclusión
Se ha creado un comportamiento de Jett que simula su rol duelista de Valorant. Jett recorre todo el mapa constantemente en busca de enemigos y limpiando las zonas potencialmente peligrosas premiando la agresividad a la estrategia, junto con su habilidad y la comunicación con su equipo.

## Pseudocódigo

function pathfindAStar(graph: Graph,
	 start: Node,
	 end: Node,
	 heuristic: Heuristic
	 ) -> Connection[]:
	 # This structure is used to keep track of the
	 # information we need for each node.
	 class NodeRecord:
	 node: Node
	 connection: Connection
	 costSoFar: float
	 estimatedTotalCost: float

	 # Initialize the record for the start node.
	 startRecord = new NodeRecord()
	 startRecord.node = start
	 startRecord.connection = null
	 startRecord.costSoFar = 0
	 startRecord.estimatedTotalCost = heuristic.estimate(start)

	 # Initialize the open and closed lists.
	 open = new PathfindingList()

	 open += startRecord
	 closed = new PathfindingList()

	 # Iterate through processing each node.
	 while length(open) > 0:
		 # Find the smallest element in the open list (using the
		 # estimatedTotalCost).
		 current = open.smallestElement()

		 # If it is the goal node, then terminate.
		 if current.node == goal:
		 	break

		 # Otherwise get its outgoing connections.
		 connections = graph.getConnections(current)

		 # Loop through each connection in turn.
		 for connection in connections:
			 # Get the cost estimate for the end node.
			 endNode = connection.getToNode()
			 endNodeCost = current.costSoFar + connection.getCost()

		 # If the node is closed we may have to skip, or remove it
		 # from the closed list.
		 if closed.contains(endNode):
			 # Here we find the record in the closed list
			 # corresponding to the endNode.
			 endNodeRecord = closed.find(endNode)

		 # If we didn’t find a shorter route, skip.
		 if endNodeRecord.costSoFar <= endNodeCost:
		 	continue

			 # Otherwise remove it from the closed list.
			 closed -= endNodeRecord

			 # We can use the node’s old cost values to calculate
			 # its heuristic without calling the possibly expensive
			 # heuristic function.
			 endNodeHeuristic = endNodeRecord.estimatedTotalCost -
			 endNodeRecord.costSoFar

			 # Skip if the node is open and we’ve not found a better
			 # route.
		 else if open.contains(endNode):
			 # Here we find the record in the open list
			 # corresponding to the endNode.

		 	endNodeRecord = open.find(endNode)

		  # If our route is no better, then skip.
		  if endNodeRecord.costSoFar <= endNodeCost:
		  	continue

			  # Again, we can calculate its heuristic.
			  endNodeHeuristic = endNodeRecord.cost -
			  endNodeRecord.costSoFar

			  # Otherwise we know we’ve got an unvisited node, so make a
			  # record for it.
		  else:
			  endNodeRecord = new NodeRecord()
			  endNodeRecord.node = endNode

			  # We’ll need to calculate the heuristic value using
			  # the function, since we don’t have an existing record
			  # to use.
			  endNodeHeuristic = heuristic.estimate(endNode)

			  # We’re here if we need to update the node. Update the
			  # cost, estimate and connection.
			  endNodeRecord.cost = endNodeCost
			  endNodeRecord.connection = connection
			  endNodeRecord.estimatedTotalCost = endNodeCost +
			 endNodeHeuristic

		 # And add it to the open list.
		 if not open.contains(endNode):
			 open += endNodeRecord

		 # We’ve finished looking at the connections for the current
		 # node, so add it to the closed list and remove it from the
		 # open list.
		 open -= current
		 closed += current

	 # We’re here if we’ve either found the goal, or if we’ve no more
	 # nodes to search, find which.
	 if current.node != goal:
	 # We’ve run out of nodes without finding the goal, so there’s
	 # no solution.
	 return null

	 else:
	 # Compile the list of connections in the path.

	 path = []

	 # Work back along the path, accumulating connections.
	 while current.node != start:
	 path += current.connection
	 current = current.connection.getFromNode()

	 # Reverse the path, and return it.
	 return reverse(path)


Utilizaremos una serie de puntos de influencia para saber que lugares son más conflictivos en los que los enemigos suelen posicionarse. 

#Loading where i've been shot before
possiblePositions = shotBefore;

#Checking places where i've been shot before
while(!possiblePositions.empty())
{
	clearPosition(possiblePositions);
}

#detecting where i've been shot from
vector3 shooterPosition = shot.shooterPosition;
possiblePositions.pushback(shooterPosition);


Utilizarempos un patron de envio de mensajes para conseguir que los personajes se comuniquen entre si.

#Sending messages
send(msg type){
	sendEnemyPosition(); // for example
}
#Recieving messages
recieve(msg type){
	if (enemyPositionType)
	{
		lookat(enemyPosition);
	}
}
