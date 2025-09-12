>Las interfaces se declaran cuando no sabemos qué tipo tendrá nuestra instancia.
Nunca dejarla en any, porque ty pierde control sobre ellas.

>CTRL + SPACE
>Hace que te aparezcan un listado de opciones y para las interfaces el un
>sonajero horizontal, con esto se importa automáticamente al la hoja .ts

`
import { Component } from '@angular/core';
==import { Character } from '../../interfaces/character.interface';==

`@Component({
  selector: 'dbz-list',
  templateUrl: './list.component.html',
  styleUrl: './list.component.css'
})
export class ListComponent {
  public characterList: ==Character==[] = [{
    name: 'Trunk',
    power: 10
  }]
}`

