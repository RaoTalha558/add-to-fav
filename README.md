html button 
<form method="POST" action="{{ route('toggle_favorite', ['id' => $ad->id]) }}">
    @csrf
    <div class="place-bid-btn">
        <button type="submit" class="btn btn-link" id="favorite-button">
            <i class="fa fa-heart @if ($isFavorite) text-danger @endif"></i>
        </button>
    </div>
</form>


route
Route::post('toggle-favorite/{id}', ['as' => 'toggle_favorite', 'uses' => 'BlogController@toggleFavorite']);

controller function 
public function toggleFavorite($id)
        {
            // Check user authentication
            if (auth()->check()) {

                // Fetch user ID using authenticated user
                $user_id = auth()->user()->id;
               
    
                // Check if the record already exists in the favorites table
                $exists = DB::table('favorites')
                    ->where('user_id', $user_id)
                    ->where('ad_id', $id)
                    ->exists();
                   
                if ($exists) {
                    DB::table('favorites')
                    ->where('user_id', $user_id)
                    ->where('ad_id', $id)
                    ->delete();
                return "Record deleted.";
                }
                
                elseif(!$exists){
                 
                    DB::table('favorites')->insert([
                        'user_id' => $user_id,
                        'ad_id' => $id,
                    ]);
                   
                    return redirect('/favourite');
                } else {
                    // If the record already exists, you can choose to remove it or do nothing
                    // Here, we'll just return a message indicating that it's already a favorite
                    return "Already in favorites.";
                }
            } else {
                return redirect('/login');
            }
        }
