import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt
import random
from datetime import datetime
relationships_path= r"C:\python\relationships.txt\relationships.txt"
relationships=pd.read_csv(
relationships_path,
sep="\t",
header=None,
names=["UserID1","UserID2"]
)
profiles_path=r"C:\python\profiles.txt\profiles.txt"
columns=[
"user_id", "public", "completion_percentage", "gender", "region", "last_login",
"registration", "AGE",
"body", "I_am_working_in_field", "spoken_languages", "hobbies",
"I_most_enjoy_good_food", "pets", "body_type",
"my_eyesight", "eye_color", "hair_color", "hair_type",
"completed_level_of_education", "favourite_color",
"relation_to_smoking", "relation_to_alcohol", "sign_in_zodiac",
"on_pokec_i_am_looking_for", "love_is_for_me",
"relation_to_casual_sex", "my_partner_should_be", "marital_status", "children",
"relation_to_children", "I_like_movies",
"I_like_watching_movie", "I_like_music", "I_mostly_like_listening_to_music",
"the_idea_of_good_evening",
"I_like_specialties_from_kitchen", "fun", "I_am_going_to_concerts",
"my_active_sports", "my_passive_sports",
"profession", "I_like_books", "life_style", "music", "cars", "politics", "relationships",
"art_culture",
"hobbies_interests", "science_technologies", "computers_internet", "education",
"sport", "movies", "travelling",
"health", "companies_brands"
]
profiles=pd.read_csv(
profiles_path,
names=columns,
sep="\t",
header=None,
index_col=False,
skipinitialspace=True,
engine="python" ,
)
profiles["user_id"] = pd.to_numeric(profiles["user_id"], errors="coerce")
def proposition():
user1= input("Enter the user ID of first person: ")
user2= input("Enter the user ID of first person: ")
user1_data = profiles.loc[profiles['user_id'] == int(user1)]
user2_data = profiles.loc[profiles['user_id'] == int(user2)]
if(user1_data.empty):
print("User not found")
else:
user1_age = user1_data['AGE'].values[0]
user1_completion_percentage = user1_data['completion_percentage'].values[0]
if(user1_age>30 and user1_completion_percentage<50 ):
print("Proposition 1 does not hold for user 1")
else:
print("Proposition 1 holds for user 1")
if(user2_data.empty):
print("User not found")
else:
user2_age = user2_data['AGE'].values[0]
user2_completion_percentage = user2_data['completion_percentage'].values[0]
if(user2_age>30 and user2_completion_percentage<50 ):
print("Proposition 1 does not hold for user 2")
else:
print("Proposition 1 holds for user 2")
user1_public = user1_data['public'].values[0]
last_login1 = user1_data['last_login'].values[0]
last_login1 = pd.to_datetime(last_login1, errors='coerce')
if (not pd.isna(last_login1)):
current_date = datetime.now()
days_since_last_login1 = (current_date - last_login1).days
if (days_since_last_login1 <= 365 and user1_public==1):
print("Proposition 2 holds for user 1")
elif(days_since_last_login1 >= 365 and user1_public==0):
print("Proposition 2 holds for user 1")
else:
print("Proposition 2 does not hold for user 1")
else:
print("Last login date is not specified by user 1!")
user2_public = user2_data['public'].values[0]
last_login2 = user2_data['last_login'].values[0]
last_login2 = pd.to_datetime(last_login2, errors='coerce')
if (not pd.isna(last_login2)):
current_date = datetime.now()
days_since_last_login2 = (current_date - last_login2).days
if (days_since_last_login2 <= 365 and user2_public==1):
print("Proposition 2 holds for user 2")
elif(days_since_last_login2 >= 365 and user2_public==0):
print("Proposition 2 holds for user 2")
else:
print("Proposition 2 does not hold for user 2")
else:
print("Last login date is not specified by user 2!")
def Quantifiers(area,age,comp_precentage):
users_in_area = profiles[
(profiles['region'].str.contains(area, case=False, na=False)) &
(profiles['AGE'] > age) &
(profiles['completion_percentage'] > comp_precentage)
]
if users_in_area.empty:
print(f"No users found in the area {area} who are above {age} and have a
completion percentage greater than {comp_precentage}.")
else:
print(f"Users found in the area: {area} who are above {age} and have a completion
percentage greater than {comp_precentage}:")
print(users_in_area[['user_id', 'region', 'AGE', 'completion_percentage']])
#sets
def sets():
A=set()
user=profiles[profiles['public']==1]
for index,user in user.iterrows():
A.add(user['user_id'])
print("set of user id of users who are public: ")
print(A)
B=set()
region=input("Enter region name: ")
ruser =profiles[profiles['region'].str.contains(region, case=False, na=False)]
if ruser.empty:
print("no user in region:",region)
else:
for index,region in ruser.iterrows():
B.add(region['user_id'])
print("set of user id of users who are in the entered region: " )
print(B)
print("A INTERSECTION B IS: ")
inter=A.intersection(B)
print(inter)
print("A UNION B IS: ")
union=A.union(B)
print(union)
universal=set()
for index,user_id in profiles.iterrows():
universal.add(user_id['user_id'])
print("UNIVERSAL SET: ")
print(universal)
comp= universal-A
print("A COMPLEMENT IS: ")
print(comp)
#counting
def counting(profiles):
c1=c2=c3=0
people1=profiles[profiles['completion_percentage']>80]
for index, row in people1.iterrows():
c1+=1
people3=profiles[
profiles['public']==1
]
for index,row in people3.iterrows():
c3+=1
c2=profiles['region'].value_counts()
print("number of people with completion_percentage above 80: ", c1)
print("number of people with public profiles: ", c3)
print(" Number of users in each region." )
print(c2)
#functions
def functions(profiles):
mapping={}
for index,i in profiles.iterrows():
userid=i['user_id']
perc=i['completion_percentage']
mapping[userid]=perc
domain=set(mapping.keys())
range_f=set(mapping.values())
codomain=set(range(0,101))
isonetoone=len(range_f) == len(domain)
isonto=codomain.issubset(range_f)
print("Domain (User IDs):", domain)
print("\nRange (Mapped Completion Percentages):", range_f)
print("\nCodomain (All Possible Completion Percentages):", codomain)
print("\nIs the mapping injective (one-to-one)?", isonetoone)
print("\nIs the mapping surjective (onto)?", isonto)
#venn diagram
def venn(profiles):
public=set()
i=profiles[profiles['public']==1]
for index,i in i.iterrows():
public.add(i['user_id'])
print("set of user id of users who are public: ")
print(public)
above30=set()
j=profiles[profiles['AGE']>30]
for index,j in j.iterrows():
above30.add(j['user_id'])
print("set of user id of users who are above 30: ")
print(above30)
sregion=set()
region=input("Enter region name: ")
k =profiles[profiles['region'].str.contains(region, case=False, na=False)]
if k.empty:
print("no user in region:",region)
else:
for index,k in k.iterrows():
sregion.add(k['user_id'])
print("set of user id of users who are in the entered region: " )
print(sregion)
publicandabove30=public.intersection(above30)
above30andregion=above30.intersection(sregion)
publicandregion=public.intersection(sregion)
allsets=public.intersection(above30,sregion)
print(" VENN DIAGRAM INTERSECTION USING CONSOLE OUTPUT: '")
print("total user who are public: ",len(public)," \nUser IDs:", public)
print("Total users above 30: ", len(above30), " \nUser IDs:", above30)
print("Total users in region ",region, len(sregion), "\n User IDs:", sregion)
print("\nOverlaps:")
print("Users who are public and above 30: ",len(publicandabove30), " \nUser IDs:",
publicandabove30)
print("Users who are public and in ",region, len(publicandregion), "\n User IDs:",
publicandregion)
print("Users who are above 30 and in ",region, len(above30andregion), "\n User IDs:",
above30andregion)
print("Users in all three sets: ", len(allsets), allsets)
print("\nUnique Counts:")
print("Public Only:", len(public - above30 - sregion), "\nUser IDs:", public - above30 -
sregion)
print("Above 30 Only:", len(above30 - public - sregion), "\n User IDs:", above30 -
public - sregion)
print("Region Only:", len(sregion - public - above30), "\n User IDs:", sregion - public -
above30)
print("\nIntersections:")
print("Public ∩ Above 30:", len(publicandabove30 - sregion), " \nUser IDs:",
publicandabove30 - sregion)
print("Public ∩ Region", region, ":", len(publicandregion - above30), "\n User IDs:",
publicandregion - above30)
print("Above 30 ∩ Region", region, ":", len(above30andregion - public), "\n User IDs:",
above30andregion - public)
print("Public ∩ Above 30 ∩ Region:", len(allsets), "\n User IDs:", allsets)
def relation1(UserID1, UserID2):
friendship_1 = ((relationships['user_A'] == UserID1) & (relationships['user_B'] ==
UserID2))
friendship_2 = ((relationships['user_A'] == UserID2) & (relationships['user_B'] ==
UserID1))
if friendship_1.any() and friendship_2.any():
print(f"The friendship relation between user {UserID1} and user {UserID2} is
symmetric.")
else:
print(f"The friendship relation between user {UserID1} and user {UserID2} is NOT
symmetric.")
def relation1():
friendships = set(zip(relationships['UserID1'], relationships['UserID2']))
symmetric = True # Assume the relation is symmetric unless proven otherwise
for UserID1, UserID2 in friendships:
if (UserID1, UserID2) not in friendships:
symmetric = False
break
if symmetric:
print("The friendship relation is symmetric.")
else:
print("The friendship relation is not symmetric.")
def relation3(user):
friendship = ((relationships['UserID1'] == user) & (relationships['UserID2'] == user))
if friendship.any() :
print(f"The friendship relation of user {user} is reflexive.")
else:
print(f"The friendship relation of user {user} is not reflexive.")
def relation3():
self_relations = set(zip(relationships['UserID1'], relationships['UserID2']))
users = set(relationships['UserID1'].unique())
missing_self_relations = [user for user in users if (user, user) not in self_relations]
if missing_self_relations:
missing_self_relations = [int(user) for user in missing_self_relations]
print(f"The friendship relation is not reflexive. Missing reflexive relations for users:
{missing_self_relations}")
else:
print("The friendship relation is reflexive.")
def relation2():
friendships = set(zip(relationships['UserID1'], relationships['UserID2']))
transitive_pairs = []
for UserID1, UserID2 in friendships:
for _, user_C in friendships:
if UserID2 == _ and (UserID1, user_C) not in friendships:
transitive_pairs.append(f"{UserID1}->{user_C}")
if transitive_pairs:
print("Transitive relations :")
for pair in transitive_pairs:
print(pair)
else:
print("No transitive relations found.")
def factorial(n):
if n == 0 or n == 1:
return 1
result = 1
for i in range(2, n + 1):
result *= i
return result
def combination_formula(n, r):
if r > n:
return 0
if r == 0 or r == n:
return 1
return factorial(n) // (factorial(r) * factorial(n - r))
def combination(age, area):
user_in_area=profiles[(profiles['region'].str.contains(area, case=False, na=False))]
users_above_age = profiles[profiles['AGE'] > age]
n = len(users_above_age)
if n == 0:
return 0
total_combinations = 0
for r in range(1, n + 1):
total_combinations += combination_formula(n, r)
return total_combinations
def permutation(user):
n = len(user)
return factorial(n)
class node:
def __init__(self, data):
self.data = data
self.children = [] #array to store multiple children
def find_node(root, data):
if root is None:
return None
if root.data == data:
return root
for child in root.children:
found = find_node(child, data)
if found:
return found
return None
def insert(root, parent_data, child_data):
if root is None:
return node(parent_data) #create the root node if it doesn't exist
parent_node = find_node(root, parent_data)
if parent_node:
#add the child node if it doesn't already exist in children
if not find_node(parent_node, child_data):
parent_node.children.append(node(child_data))
return root
def preorder(root):
if root is not None:
print(root.data, end=" ")
for child in root.children:
preorder(child)
def postorder(root):
if root is not None:
for child in root.children:
postorder(child)
print(root.data, end=" ")
def breadth_first_traversal(root):
if root is None:
return
queue = [root]
while queue: #while the queue is not empty
node = queue.pop(0)
print(node.data, end=" ")
queue.extend(node.children) #add the children of the current node to the queue
def create_tree_of_friends(userID, relationships):
root = Nonefor _, row in relationships.iterrows():
user = row["UserID1"]
friend = row["UserID2"]
if user == userID:
if root is None:
root = node(user) # Create the root node
root = insert(root, user, friend)
elif root is not None: # If we've already created the tree for the user, stop
break
return root
def create_graph(relationships):
graph=nx.Graph()
for index, row in relationships.iterrows():
user = row["UserID1"]
friend = row["UserID2"]
graph.add_edge(user, friend)
return graph
def extract_subgraph(graph, num_of_nodes):
subgraph = nx.Graph()
nodes = list(graph.nodes)[:num_of_nodes]
for node in nodes:
for neighbor in graph.neighbors(node):
if neighbor in nodes:
subgraph.add_edge(node, neighbor)
return subgraph
def check_bipartite(graph):
return nx.is_bipartite(graph)
def create_mst(sub_graph):
for u, v in sub_graph.edges:
sub_graph[u][v]["weight"] = random.randint(1, 100)
# Use Kruskal's algorithm to calculate MST
mst = nx.minimum_spanning_tree(sub_graph, weight="weight")
return mst
graph=create_graph(relationships)
subgraph = extract_subgraph(graph, 3000)
while True:
print("\n========= MENU =========")
print("1. Proposition Function")
print("2. Quantifiers Function")
print("3. Sets Function")
print("4. Venn Diagram Function")
print("5. General Function")
print("6. Relation 1")
print("7. Relation 2")
print("8. Relation 3")
print("9. Permutation Function")
print("10. Combination Function")
print("13. Counting function")
print("14. tree function")
print("15. graph social network")
print("16. bipartite of a subgragh")
print("17. MST")
print("18. print first few lines of profiles ")
print("19. print first few lines of relatioships")
print("-1. Exit Program")
print("========================")
choice = int(input("Enter your choice (enter -1 to exit): "))
if choice == -1:
print("Exiting the program. Goodbye!")
break
elif choice == 1:
proposition()
elif choice == 2:
comp_precentage=50
area= input("enter region for quamtifiers: \n")
age=18
Quantifiers(area, age, comp_precentage)
elif choice == 3:
sets()
elif choice == 4:
venn(profiles)
elif choice == 5:
functions(profiles)
elif choice == 6:
relation1()
elif choice == 7:
relation2()
elif choice == 8:
relation3()
elif choice == 9:
sorted = profiles.sort_values(by='completion_percentage', ascending=False)
num_permutations = permutation(sorted)
print("number of permutations: ",num_permutations)
elif choice == 10:
a=int(input("enter age for combination func: " ))
ar=input("enter area for combination function: ")
result=combination(a,ar)
print("total combination of users above region",ar,"and age ",a,"is: ",result)
elif choice == 13:
counting(profiles)
elif choice==14:
u=int(input(" enter user id for tree function: "))
tree = create_tree_of_friends(u, relationships)
print("Preorder Traversal of the Tree of Friends:")
print(preorder(tree))
print("\nPostorder Traversal of the Tree of Friends:")
postorder(tree)
print("\nBreadth-First Traversal of the Tree of Friends:")
breadth_first_traversal(tree)
elif choice==15:
print("Representing the social network as a graph where nodes are users and
edges are friendships: ")
plt.figure(figsize=(10,10))
nx.draw(graph, with_labels=False, edge_color='lightblue', node_color='blue',
node_size=10)
plt.title("Social Network Graph")
plt.show()
elif choice==16:
print("Extracting a bipartite subgraph ")
is_bipartite = check_bipartite(subgraph)
print("Is the subgraph bipartite?,")
if is_bipartite:
print ("yes")
color_map = []
set1, set2 = nx.bipartite.sets(subgraph)
for node in subgraph.nodes():
if node in set1:
color_map.append('lightblue')
else:
color_map.append('lightgreen')
nx.draw(graph, with_labels=False, edge_color='lightblue', node_color='blue',
node_size=10)
plt.title("Bipartite Subgraph")
plt.show()
else:
print("No")
elif choice==17:
print("Calculating Minimum Spanning Tree (MST) for a subgraph (3000 nodes):")
subgraph = extract_subgraph(graph, 3000)
mst = create_mst(subgraph)
print("Displaying MST of the subgraph:")
nx.draw(subgraph, with_labels=False, edge_color='lightblue', node_color='blue',
node_size=10)
plt.title("Minimum Spanning Tree")
plt.show( 
elif choice == 18:
print("first few lines of profiles txt is as follows: ")
print (profiles.head())
elif choice == 19:
print("first few lines of relationships txt is as follows: ")
print (relationships.head())
else:
print("Invalid choice! Please select a valid option from the menu.")
