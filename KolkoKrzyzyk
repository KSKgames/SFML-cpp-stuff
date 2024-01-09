//*Projekt zawierający język Polski*
//Bazowane na przykładzie kółka z https://sfml-dev.org 
//Zrobione z pomocą wielu innych rzeczy z tejże strony
#include <SFML/Graphics.hpp> 
struct pos { int x; int y; }; //struktury bo czemu nie
struct rect { int x1; int y1; int x2; int y2; };

//kosmetyka
const int shapeSize = 70, 
          shapeThickness = 10;
const unsigned int crossColor[3] = { 252,75,66 }, 
                   circleColor[3] = { 59,183,255 };
//pola w grze
const rect Pola[9] = {{0,0,100,100},{100,0,200,100},{200,0,300,100},
                    {0,100,100,200},{100,100,200,200},{200,100,300,200},
                    {0,200,100,300},{100,200,200,300},{200,200,300,300} };
const short int Wygrane[8][3] = { {0,1,2},{3,4,5},{6,7,8},{0,3,6},{1,4,7},{2,5,8},{0,4,8},{2,4,6} };

short int ruch = 0;
short int Plansza[9] = {0};
short int Wygrana = -1;

int ZnajdzPole(pos p){
    for(int i=0;i<9;i++){
        if(p.x >= Pola[i].x1 && p.x < Pola[i].x2 && p.y >= Pola[i].y1 && p.y < Pola[i].y2){
            return i;
        }
    }
    return 0;
}

bool SprawdzWygrana(short int p){
    for(int i=0;i<8;i++){
        if(Plansza[Wygrane[i][0]] == p && Plansza[Wygrane[i][1]] == p && Plansza[Wygrane[i][2]] == p){
            Wygrana = i;
            return true;
        }
    }
    return false;
}

sf::RectangleShape RysujWygraj(){
    int dlugosc =Pola[2].x2;
    rect DobrePole = Pola[Wygrane[Wygrana][0]];
    sf::RectangleShape line;
    line.setFillColor(sf::Color::Black);
    if(Wygrana >= 0 && Wygrana <= 2){
        line.setOrigin(0, shapeThickness/2);
        line.setSize(sf::Vector2f(dlugosc, shapeThickness));
        line.setPosition(0,(DobrePole.y1 + DobrePole.y2)/2);
    }
    else if (Wygrana>=3 && Wygrana<=5) {
        line.setOrigin(shapeThickness/2,0);
        line.setSize(sf::Vector2f(shapeThickness, dlugosc));
        line.setPosition((DobrePole.x1+DobrePole.x2)/2,0);
    }
    else {
        line.setOrigin(0, shapeThickness / 2);
        line.rotate(45);
        line.setSize(sf::Vector2f(dlugosc*1.4142, shapeThickness));
        if(Wygrana==7) {
            line.setPosition(0, dlugosc);
            line.rotate(-90);
        }
    }
    return line;
}

int main(){
    //okno
    sf::RenderWindow window(sf::VideoMode(300, 300), "Kólko i Krzyzyk");
    //definowanie kółka
    sf::CircleShape circle(shapeSize / 2);
    circle.setFillColor(sf::Color::Transparent);
    circle.setOutlineThickness(shapeThickness);
    circle.setOutlineColor(sf::Color(crossColor[0], crossColor[1], crossColor[2]));
    circle.setOrigin(shapeSize / 2, shapeSize / 2);
    //definiowanie krzyżyka
    sf::RectangleShape crossHalf(sf::Vector2f(shapeSize * 1.4142, shapeThickness));
    crossHalf.setOrigin(shapeSize / 2 * 1.4142, shapeThickness / 2);
    crossHalf.rotate(-45.f);
    crossHalf.setFillColor(sf::Color(circleColor[0], circleColor[1], circleColor[2]));
    sf::RectangleShape pasekWygranej;

    //window loop
    while(window.isOpen()){
        //eventy
        sf::Event event;
        while(window.pollEvent(event)){
            switch(event.type) {
            case sf::Event::Closed:
                window.close();
                break;
            case sf::Event::MouseButtonPressed:
                if(event.mouseButton.button == sf::Mouse::Left && Wygrana==-1) {
                    int id = ZnajdzPole({ event.mouseButton.x,event.mouseButton.y });
                    if(Plansza[id] == 0){
                        Plansza[id] = (ruch % 2) + 1;
                        if(SprawdzWygrana(Plansza[id])){
                            pasekWygranej = RysujWygraj();
                        }
                        ruch++;
                    }
                }
                break;
            }
        }

        //rysowanie
        window.clear(sf::Color::White);
        for (int i=0;i<9;i++){
            if (Plansza[i] == 1){
                crossHalf.setPosition((Pola[i].x1 + Pola[i].x2) / 2, (Pola[i].y1 + Pola[i].y2) / 2);
                window.draw(crossHalf);
                crossHalf.rotate(90);
                window.draw(crossHalf);
                crossHalf.rotate(-90);
            }
            else if (Plansza[i] == 2){
                circle.setPosition((Pola[i].x1 + Pola[i].x2) / 2, (Pola[i].y1 + Pola[i].y2) / 2);
                window.draw(circle);
            }
        }
        if(Wygrana != -1){
            window.draw(pasekWygranej);
        }
        window.display();
    }
    return 0;
}
