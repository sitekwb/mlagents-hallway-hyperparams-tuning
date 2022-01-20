# Testowanie hiperparametrów agenta Hallway Unity
### Autorzy: Dawid Brzozowski, Wojciech Sitek
### Informacje o projekcie: Przedmiot Uczące się Systemy Decyzyjne 2021Z, studia magisterskie na kierunku Informatyka ze specjalnością Sztuczna inteligencja
### Afiliacja: Instytut Informatyki, Wydział Elektroniki i Technik Informacyjnych, Politechnika Warszawska
### Prowadzący projekt: mgr inż. Łukasz Lepak

## Instrukcja instalacji Unity

Podczas instalacji środowiska ml-agents na systemie Linux oraz MacOS (w dalszej części raportu będziemy korzystać z systemu Linux), wykorzystano informacje znajdujące się w repozytorium GitHub projektu ML-Agents. W celu instalacji środowiska w systemie Linux wykonano następujące kroki:
- Ze strony https://unity3d.com/get-unity/download pobraliśmy aplikację Unity Hub.
- Po uruchomieniu aplikacji Unity Hub, pobraliśmy Edytor Unity w wersji 2019.4.34f1, zgodnej z wersją środowiska ml-agents.
- Sklonowaliśmy repozytorium https://github.com/Unity-Technologies/ml-agents/ z gałęzią release_19. (do folderu ~/Documents/PW/USD/)
- W aplikacji Unity Hub rozwinęliśmy listę przy przycisku Open, kliknęliśmy w “Add from the disk”, następnie wskazaliśmy katalog ml-agents/Project jako katalog główny projektu do załadowania.
- Po załadowaniu projektu otworzyliśmy go bez błędów kompilacji.
- Otwieraliśmy różne przygotowane wcześniej sceny przykładowe z katalogu ml-agents/Project/Assets/ML-Agents/Examples/<Nazwa przykładu>/Scenes/<Nazwa przykładu>.unity z poziomu środowiska Unity.
- Wykorzystując pretrenowane przykładowe modele, testowaliśmy działanie środowisk Basic, Hallway oraz kilku innych. Agenci w pretrenowanych przykładowych środowiskach w większości przypadków wypełniali powierzone zadanie poprawnie.

## Instrukcja instalacji Python

Do projektu wykorzystano zainstalowaną wcześniej dystrybucję programu Anaconda do wirtualizacji środowiska języka Python. Do instalacji środowiska wykonano następujące działania:
- Utworzyliśmy środowisko wirtualne za pomocą komendy: `conda create -n mlagents python=3.9`.
- Aktywowaliśmy środowisko za pomocą komendy: `conda activate mlagents`.
- W katalogu USD utworzyliśmy folder eksperymentalny exp (komenda `mkdir`) i ustawiliśmy go na katalog powłoki (komenda `cd`).
- Skopiowaliśmy domyślne ustawienia hiperparametrów projektu Hallway z katalogu projektowego za pomocą komendy: `cp ~/Documents/PW/USD/ml-agents/config/ppo/Hallway.yaml ./ppo_hallway.yaml && cp ~/Documents/PW/USD/ml-agents/config/sac/Hallway.yaml ./sac_hallway.yaml`
- Przestudiowaliśmy znaczenie działania hiperparametrów w algorytmach PPO oraz SAC, przy użyciu źródeł przedmiotowych oraz instrukcji z projektu ML-Agents (https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-Configuration-File.md) oraz wybraliśmy kilka hiperparametrów dla każdego algorytmu, których wpływ na trenowanie chcielibyśmy przetestować.
- Skopiowaliśmy pliki z domyślną konfiguracją liczbę razy równą liczbie permutacji wybranych wartości wybranych parametrów, za pomocą komendy: `cp <ppo/sac>_hallway.yaml <ppo/sac>_<liczba lub nazwa>.yaml`.
- Dla każdego pliku zmieniliśmy wartości wybranych parametrów na ustalone wcześniej permutacje.
- Zainstalowaliśmy dwa pakiety niezbędne do trenowania agenta środowiska Hallway za pomocą GPU (posiadany sprzęt to NVIDIA Palit RTX 3070Ti) za pomocą komend: pip install mlagents (zainstalowana u nas wersja to 0.27.0) oraz conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c conda-forge. W naszym przypadku, niezbędne było dodatkowe instalowanie dodatku CUDA oraz sterowników NVIDIA (dokładnie nvidia-driver-470). Jednak zastrzegamy, że te komendy mogą różnić się w zależności od sprzętu i systemu operacyjnego. Zależało nam, aby trenowanie odbywało się z użyciem GPU. Niniejsze pakiety oraz ich podzależności są zapisane w naszym repozytorium w pliku environment.yml.
- Dla każdego przygotowanego wcześniej pliku z hiperparametrami uruchamialiśmy następujący zestaw akcji:
  - Komenda: `mlagents-learn ~/Documents/PW/USD/exp/<ppo/sac>_<liczba lub nazwa>.yaml –run-id=<ppo/sac>_<liczba lub nazwa>`
  - Po pojawieniu się zachęty do uruchomienia środowiska Unity w postaci (“Listening on port 5004. Start training by pressing the Play button in the Unity Editor”), przechodziliśmy do środowiska Unity i mając otwarte środowisko Hallway, kilkaliśmy przycisk Play.
  - W środowisku Unity było widać zmagania agenta w celu rozpoznania środowiska, a następnie dokonywania poprawnych wyborów.
  - W konsoli pojawiały się informacje na temat statystyk kolejnych kroków (numer kroku, czas, średnia nagroda, odchylenie standardowe nagrody).
  - Co oznaczoną w hiperparametrach wielkość (10.000 kroków) program zapisywał dotychczasowy model, pojawiała się także informacja.
  - Po wyczerpanej liczbie kroków, także określonej w hiperparametrach, algorytm zapisywał model i statystyki do plików i kończył pracę. Można było wtedy uruchomić kolejny algorytm.
  - Uczenie wszystkich parametrów zajęło około 2 tygodni (ponieważ nie zawsze orientowaliśmy się od razu, że trzeba uruchomić kolejny zestaw trenujący)
  - Istnieje specjalna biblioteka ml-agents-hyperparams (https://github.com/mbaske/ml-agents-hyperparams) do uruchamiania wielu procesów trenowania równolegle, jednak jest ona dostępna tylko na system Windows, który nie był zainstalowany na naszych urządzeniach. Było nam więc mimo wszystko wygodniej uruchamiać wszystkie zestawy hiperparametrów ręcznie w systemie Linux, dlatego że ich liczba nie była aż tak duża.
  - Wyniki działania trenowania zostały zapisane w katalogu exp/results, który został umieszczony w naszym repozytorium.
  
  ## Dokumentacja
  Pełna dokumentacja projektu znajduje się w katalogu `docs`.
