 @doc """
  Determines the Ecto SSL mode based on the ECTO_SSL_MODE environment variable, the
  sslmode parameter in the database URL, or defaults to "require" if neither is set.
  """
  @spec ecto_ssl_mode(database_url :: String.t() | nil) :: String.t()
  def ecto_ssl_mode(database_url \\ nil), do: ecto_ssl_mode(database_url, &System.get_env/1)

  @spec ecto_ssl_mode(database_url :: String.t() | nil, env_function :: (String.t() -> String.t() | nil)) :: String.t()
  def ecto_ssl_mode(database_url, env_function) do
    mode =
      blank_to_nil(env_function.("ECTO_SSL_MODE")) ||
        ssl_mode_from_database_url(database_url) ||
        "require"
