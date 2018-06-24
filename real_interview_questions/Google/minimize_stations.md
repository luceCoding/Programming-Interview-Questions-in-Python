```
import heapq

def place_stations(n_new_stations, len_road, stations):
    sorted_stations = stations.sort()
    start_index = 0
    heap = list()
    for element in stations:
        distance = element - start_index
        if distance != 0:
            node = (distance, 0, distance)
            heapq.heappush(heap, node)
        start_index = element + 1
    distance = 0
    if len(stations) != 0:
        distance = start_index - stations[-1]
    else:
        distance = start_index - len_road
    if distance != 0:
        node = (distance, 0, distance)
        heapq.heappush(heap, node)
    n_stations_placed = 0
    print heap
    
    while len(heap) != 0 and n_new_stations != 0:
        longest_road = heapq.heappop(heap)
        new_road = place_station_on_road(longest_road)
        if new_road[0] != 0:
            heapq.heappush(heap, new_road)
        n_new_stations -= 1
        n_stations_placed += 1
    return n_stations_placed
    
def place_station_on_road(road):
    distance_between = road[0]
    n_stations_on_road = road[1]
    length_of_road = road[2]
    
    n_stations_on_road += 1
    if n_stations_on_road == length_of_road:
        return (0, n_stations_on_road, length_of_road)
    distance_between = float(n_stations_on_road / length_of_road)
    return (distance_between, n_stations_on_road, length_of_road)
    
    
N = 5
M = 20
K = 3
T = [3, 7, 15, 18, 1]

print place_stations(K, M, T)
```
