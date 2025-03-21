#include <iostream>
#include <string>

using namespace std;

struct Song {
    string title;
    Song* next;

    Song(string t) {
        title = t;
        next = nullptr;
    }
};

class Playlist {
private:
    Song* last;  

public:
    Playlist() {
        last = nullptr;
    }

   
    void addSong(string songTitle) {
        Song* newSong = new Song(songTitle);

        if (last == nullptr) {  
            last = newSong;
            last->next = last;  
        } else {
            newSong->next = last->next;
            last->next = newSong;
            last = newSong;
        }
        cout << "Added: " << songTitle << " to the playlist."<< endl;
    }


    void deleteSong(string songTitle) {
        if (last == nullptr) {
            cout << "Playlist is empty."<< endl;
            return;
        }

        Song *current = last->next, *prev = last;

        do {
            if (current->title == songTitle) {  
                if (current == last && current->next == last) {  
                    last = nullptr;
                } else {
                    prev->next = current->next;
                    if (current == last) {  
                        last = prev;
                    }
                }
                delete current;
                cout << "Deleted: " << songTitle << " from the playlist."<< endl;
                return;
            }
            prev = current;
            current = current->next;
        } while (current != last->next);

        cout << "Song not found in the playlist."<< endl;
    }

 
    void displayPlaylist() {
        if (last == nullptr) {
            cout << "Playlist is empty."<< endl;
            return;
        }

        Song* temp = last->next;
        cout << "Current Playlist:"<<endl;
        do {
            cout << temp->title << " -> ";
            temp = temp->next;
        } while (temp != last->next);
        cout << "(Back to start)"<< endl;
    }

    void nextSong() {
        if (last == nullptr) {
            cout << "Playlist is empty."<< endl;
            return;
        }

        last = last->next;
        cout << "Now playing: " << last->next->title << endl;
    }
};

int main() {
    Playlist myPlaylist;
    int choice;
    string songTitle;

    do {
        cout << "Playlist Menu:"<<endl;
        cout << "1. Add Song"<<endl;
        cout << "2. Delete Song"<<endl;
        cout << "3. Display Playlist"<<endl;
        cout << "4. Play Next Song"<<endl;
        cout << "5. Exit"<<endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                cout << "Enter song title: ";
                getline(cin, songTitle);
                myPlaylist.addSong(songTitle);
                break;
            case 2:
                cout << "Enter song title to delete: ";
                getline(cin, songTitle);
                myPlaylist.deleteSong(songTitle);
                break;
            case 3:
                myPlaylist.displayPlaylist();
                break;
            case 4:
                myPlaylist.nextSong();
                break;
            case 5:
                cout << "Exiting playlist"<<endl;
                break;
            default:
                cout << "Invalid choice. Try again."<<endl;
        }
    } while (choice != 5);

    return 0;
}
