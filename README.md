"""
Игра в «пьяницу». В игре в «пьяницу» карточная колода раздается поровну
двум игрокам. Далее они вскрывают по одной верхней карте, и тот, чья карта
старше, забирает себе обе вскрытые карты, которые кладутся под низ его
колоды. Тот, кто остается без карт – проигрывает. Для простоты будем
считать, что все карты различны по номиналу, а также, что самая младшая
карта побеждает самую старшую карту ("шестерка берет туза"). Игрок,
который забирает себе карты, сначала кладет под низ своей колоды карту
первого игрока, затем карту второго игрока (то есть карта второго игрока
оказывается внизу колоды). Напишите программу, которая моделирует игру в
пьяницу и определяет, кто выигрывает.
В игре участвует 10 карт, имеющих значения от 0 до 9, большая карта
побеждает меньшую, карта со значением 0 побеждает карту 9.
Входные данные Программа получает на вход две строки: первая строка
содержит 5 чисел, разделенных пробелами—номера карт первого игрока,
вторая – аналогично 5 карт второго игрока. Карты перечислены сверху вниз,
то есть каждая строка начинается с той карты, которая будет открыта первой.
Выходные данные. Программа должна определить, кто выигрывает при
данной раздаче, и вывести слово first или second, после чего вывести
количество ходов, сделанных до выигрыша. Если на протяжении 10^6 ходов
игра не заканчивается, программа должна вывести слово botva.
Примеры входные данные 1 3 5 7 9 2 4 6 8 0 выходные данные second 5
"""

from typing import List

class Queue:
    """Класс реализует очередь для хранения карт."""
    def __init__(self, items: List[int]) -> None:
        self.items = items

    def enqueue(self, item: int) -> None:
        self.items.append(item)

    def dequeue(self) -> int:
        if self.is_empty():
            raise IndexError("Очередь пуста")
        return self.items.pop(0)

    def is_empty(self) -> bool:
        return len(self.items) == 0

    def size(self) -> int:
        return len(self.items)

def compare_cards(card1: int, card2: int) -> int:
    if card1 == 0 and card2 == 9:
        return 1
    elif card1 == 9 and card2 == 0:
        return -1
    elif card1 < card2:
        return 1
    elif card1 > card2:
        return -1
    else:
        return 0

def play_game(player1_cards: List[int], player2_cards: List[int]) -> str:
    """
    Моделирует игру и возвращает результат.
    """
    
    queue1 = Queue(player1_cards)
    queue2 = Queue(player2_cards)
    
    max_moves = 10**6
    moves = 0
    
    while not queue1.is_empty() and not queue2.is_empty() and moves < max_moves:
        moves += 1
        card1 = queue1.dequeue()
        card2 = queue2.dequeue()
        
        result = compare_cards(card1, card2)
        
        if result == 1:
            queue1.enqueue(card1)
            queue1.enqueue(card2)
        else:
            queue2.enqueue(card1)
            queue2.enqueue(card2)
    
    if queue1.is_empty():
        return f"second {moves}"
    elif queue2.is_empty():
        return f"first {moves}"
    else:
        return "botva"

def run_game():
    print("\nВведите карты первого игрока (5 чисел от 0 до 9 через пробел):")
    while True:
        try:
            first_player_input = input()
            first_player_cards = list(map(int, first_player_input.strip().split()))
            if len(first_player_cards) != 5:
                print("Ошибка: необходимо ввести ровно 5 чисел.")
                continue
            if not all(0 <= c <= 9 for c in first_player_cards):
                print("Ошибка: карты должны быть числами от 0 до 9.")
                continue
            break
        except ValueError:
            print("Ошибка ввода. Убедитесь, что вводите числа через пробел.")

    print("\nВведите карты второго игрока (5 чисел от 0 до 9 через пробел):")
    while True:
        try:
            second_player_input = input()
            second_player_cards = list(map(int, second_player_input.strip().split()))
            if len(second_player_cards) !=5:
                print("Ошибка: необходимо ввести ровно 5 чисел.")
                continue
            if not all(0 <= c <=9 for c in second_player_cards):
                print("Ошибка: карты должны быть числами от 0 до 9.")
                continue
            set_p1 = set(first_player_cards)
            set_p2 = set(second_player_cards)
            if set_p1.intersection(set_p2):
                print("Ошибка: у обоих игроков есть одинаковые карты. Введите уникальные карты для каждого.")
                continue
            break
        except ValueError:
            print("Ошибка ввода. Убедитесь, что вводите числа через пробел.")

    result = play_game(first_player_cards, second_player_cards)

    print("\nРезультат игры:")
    if result == "botva":
        print("botva")
    else:
        winner, moves_count = result.split()
        print(f"{winner} выиграл за {moves_count} ходов.")

def main():
    while True:
        print("\n===Добро пожаловать в игру <<Пьяница>>===")
        print("1. Запустить новую игру")
        print("2. Выйти из программы")
        choice = input("Выберите действие (1-2): ")

        if choice == '1':
            run_game()
        elif choice == '2':
            print("Выход из программы.")
            break
        else:
            print("Некорректный выбор. Попробуйте снова.")

if __name__ == "__main__":
    main()

