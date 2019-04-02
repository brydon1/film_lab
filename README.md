#Film Project

#Create a list of titles in first line
filmFile = "Film.csv"
file = open(filmFile, "r")
titles = file.readline().strip()
titles = titles.split(";")
file.close()

def listAllFilms(filmFile):
    "List the names of the films with each numbered, ignore the first two lines"
    films=open(filmFile,"r",encoding='ISO-8859-1') #r is read only
    i=0
    for film in films: #Considers each line in file one at a time, top to bottom
        if i>1: #Excludes first two rows
            film=film.strip() # Get rid of "\n"
            parsed=film.split(";") #Creates list of words
            print(str(i-1)+". " + parsed[2]) #Index 2 is title
        i+=1
    films.close

def numMovies(num):
  if num == 1:
    print("\nThere was 1 movie.")
  else:
    print("\nThere were", num, "movies.")

def viewByYear(fileName):
    "List the film name and year for each film"
    year=int(input("\nWhat year films would you like to see? "))
    num = 0
    films=open(fileName,"r",encoding='ISO-8859-1')
    for film in films:
        parsed = film.split(";") 
        if str(year) == parsed[0]: #This index gives year
            num += 1
            displayFilm(parsed)
    numMovies(num)
    films.close()

def displayFilm(film):
    "Display the info on a film."
    for i in [2,0,1,3,4,5,6,7]: #Starts with title of movie
        if film[i] == "": #If entry empty
            print(titles[i] + ": Unknown" )
        else: #If entry not empty
            print(titles[i] + ": " + film[i])
    print()

def numPerYear(fileName):
    "Creates list of number of movies produced in each year"
    file = open(fileName, "r")
    years = []
    i = 0
    for line in file:
        if i > 1: #Not including first two lines
            line = line.strip()
            parsed = line.split(";")
            years.append(int(parsed[0])) #Add year to list years
        i += 1
    print("Year | Count")
    for j in range(1920,1998):
        print(str(j) + ": " + str(years.count(j))) #Shows number of occurances for each year
    file.close()

def getSubject():
    "Determines which subject a user wants"
    subjects = ["Comedy","Horror","Action","Drama","Mystery","Science Fiction","Music","War","Westerns","Western","Short","Adventure","Crime","Romance","Fantasy"]
    response = 0
    print()
    while not (response < 16 and response > 0): #When response outside of range
        for name in subjects:
            print(str(subjects.index(name) + 1) + ". " + name)
        response = input("\nEnter a subject number. ")
        if response.isdigit():
            response = int(response)
        else:
            response = 0
    print(subjects[response - 1])
    return subjects[response - 1] #returns indicated subject

def listBySubject(fileName):
    "Displays list of movies by subject for given fileName"
    file = open(fileName, "r")
    subject = getSubject()
    num = 0
    for line in file:
        line = line.strip()
        parsed = line.split(";")
        if parsed[3] == subject: #if the subject of a movie is the selected one
            displayFilm(parsed)
            num += 1
    numMovies(num)
    file.close()

def listByActor(fileName):
    "Displays list of movies containing partial matches to given actor name"
    actor = input("\nWhat actor do you want to see movies for? ")
    file = open(fileName,"r")
    num = 0
    for line in file:
        line = line.strip().split(";") #removes whitespace and creates list of words
        actors = line[4] + line[5] #string of actor and actress names
        if " " in actor: #If there are several words in input
            actor = actor.split(" ") #create list of words
            for name in actor: #if any word in actor found in actors
                if name.lower() in actors.lower(): 
                    displayFilm(line)
                    num += 1
                    break
        else: #if only one word in input
            if actor.lower() in actors.lower():
                displayFilm(line)
                num += 1
    numMovies(num)
    file.close()

def ask():
    "Prompts user to indicate preference for program"
    response = input("""\nSelect:
  1. List by year
  2. List by subject
  3. List by actor
  4. Exit\n""")
    return response

def main(fileName):
    print("Welcome to the film info directory.")
    response = ask()
    while response != '4':
        if response == '1':
          try:
            viewByYear(fileName)
            response = ask()
          except ValueError:
            print("Please enter a valid year. ")
            continue
        elif response == '2':
            listBySubject(fileName)
            response = ask()
        elif response == '3':
            listByActor(fileName)
            response = ask()
        elif response == '4':
            break
        else:
            response = ask()

main(filmFile)





