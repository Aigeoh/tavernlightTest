# tavernlightTest
questions 1 - 4


1:

local StorageKey = 1000  --use variables instead of straight numbers, easier to modify on the fly and or add some kinda detection method to fine tune the most efficient numbers
local ReleaseStorageDelay = 1000
local function releaseStorage(player)
    player:setStorageValue(StorageKey, -1) -- i dont have acess to this method, so i can only assume the storage value has something to do with saving the inventory on logout
    end
    
    function onLogout(player)
    if player:getStorageValue(StorageKey) == 1 then       -- if the storage key is 1 then call releaseStorage method with the delay from the variable declared earlier (1000) and targeting the current player that is logging out (i assume)
    addEvent(releaseStorage, ReleaseStorageDelay, player)
    end
    return true
    end


2: 

function printSmallGuildNames(memberCount)
    -- Prepare the query with certain input
    local selectGuildQuery = "SELECT name FROM guilds WHERE max_members < ?;"
    
    -- run the query with the provided memberCount
    local result = db.storeQuery(selectGuildQuery, memberCount)

    -- Check if the query came back correctly
    if result ~= nil then
        -- run through the set and print the names (if it has one)
        repeat
            local guildName = result.getString(result, "name") 
            if guildName ~= nil then
                print(guildName)
            end
        until not result.next(result)

        
        result.free(result)
    else
        print("Error executing database query.")  -- catches it if theres an error
    end
end



3:

function doSthWithPlayerParty(playerId, membername)
    local player = Player(playerId)
    local v = party.getMembers.count   -- Gave v a local variable equal to the party member count
    
    if player then -- Checks if there is a player
        local party = player:getParty()
        
        -- Check if player is in a party
        if party then
            local members = party:getMembers()
            
            -- Iterate through party members
            for v, member in pairs(members) do   --I removed k as it wasn't doing anything
                -- Check if the member's name matches the provided membername
                if member:getName() == membername then
                    -- Remove the member from the party
                    party:removeMember(member)
                    return true -- Exit the function after removing the member
                end
            end
            -- If the loop completes without finding the member, it means the member is not in the party
            print("Member is not in the party.")
            return false
        else
            print("Player is not in a party.")
            return false
        end
    else
        print("Player does not exist.")
        return false
    end
end



4:

void Game::addItemToPlayer(const std::string& recipient, uint16_t itemId)
{
Player* player = g_game.getPlayerByName(recipient);
Player* player = g_game.getPlayerByName(recipient);
if (!player) {
    player = new Player(nullptr);    -- creates a new player if there isnt one
    if (!IOLoginData::loadPlayerByName(player, recipient)) {
        delete player; -- Clean up the dynamically allocated memory, before hand if the check failed it wouldnt clean up the player that was created from the previous if statement
        return;
    }
}


Item* item = Item::CreateItem(itemId);
if (!item) {
return;
}

g_game.internalAddItem(player->getInbox(), item, INDEX_WHEREEVER, FLAG_NOLIMIT);

if (player->isOffline()) {
IOLoginData::savePlayer(player);
}
}